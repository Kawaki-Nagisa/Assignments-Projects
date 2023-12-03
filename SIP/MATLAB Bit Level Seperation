% Read the original image
originalImage = imread('new_image.jpg');

% Convert the image to grayscale if it's a color image
if size(originalImage, 3) == 3
    grayscaleImage = rgb2gray(originalImage);
else
    grayscaleImage = originalImage;
end

% Display the original image
figure;
subplot(3, 3, 1);
imshow(grayscaleImage);
title('Original Image');

% Separate the image into 8-bit levels and display each level
for i = 1:8
    bitLevel = bitget(grayscaleImage, i);
    subplot(3, 3, i+1);
    imshow(bitLevel, []);
    title(['Bit Level ' num2str(i)]);
end

% Separate the image into 8-bit levels
newImageTop = uint8(0);
newImageBottom = uint8(0);
for i = 5:8
    newImageTop = newImageTop + bitget(grayscaleImage, i) * 2^(i-1);
end
for i = 1:4
    newImageBottom = newImageBottom + bitget(grayscaleImage, i) * 2^(i-1);
end

% Resize the images to the same size
newImageTop = imresize(newImageTop, size(grayscaleImage));
newImageBottom = imresize(newImageBottom, size(grayscaleImage));

% Display the combined images
figure;
subplot(1, 3, 1);
imshow(grayscaleImage);
title('Original Image');

subplot(1, 3, 2);
imshow(newImageTop, []);
title('Top 4 Image');

subplot(1, 3, 3);
imshow(newImageBottom, []);
title('Bottom 4 Image');

% Calculate image quality metrics for the top 4 bit levels
psnrValueTop = psnr(newImageTop, grayscaleImage);
euclideanErrorTop = norm(double(newImageTop(:)) - double(grayscaleImage(:)));
mseValueTop = immse(newImageTop, grayscaleImage);
rmseValueTop = sqrt(mseValueTop);
wpsnrValueTop = psnr(newImageTop, grayscaleImage, max(grayscaleImage(:)));

% Display metrics values for the top 4 bit levels
fprintf('Top 4 Bits - PSNR: %.2f dB\n', psnrValueTop);
fprintf('Top 4 Bits - Euclidean Error: %.2f\n', euclideanErrorTop);
fprintf('Top 4 Bits - MSE: %.2f\n', mseValueTop);
fprintf('Top 4 Bits - RMSE: %.2f\n', rmseValueTop);
fprintf('Top 4 Bits - WPSNR: %.2f dB\n', wpsnrValueTop);

% Calculate image quality metrics for the bottom 4 bit levels
psnrValueBottom = psnr(newImageBottom, grayscaleImage);
euclideanErrorBottom = norm(double(newImageTop(:)) - double(grayscaleImage(:)));
mseValueBottom = immse(newImageBottom, grayscaleImage);
rmseValueBottom = sqrt(mseValueBottom);
wpsnrValueBottom = psnr(newImageBottom, grayscaleImage, max(grayscaleImage(:)));

% Display metrics values for the bottom 4 bit levels
fprintf('Bottom 4 Bits - PSNR: %.2f dB\n', psnrValueBottom);
fprintf('Bottom 4 Bits - Euclidean Error: %.2f\n', euclideanErrorBottom);
fprintf('Bottom 4 Bits - MSE: %.2f\n', mseValueBottom);
fprintf('Bottom 4 Bits - RMSE: %.2f\n', rmseValueBottom);
fprintf('Bottom 4 Bits - WPSNR: %.2f dB\n', wpsnrValueBottom);

% Construct tables for the values
tableValuesTop = table(psnrValueTop, euclideanErrorTop, mseValueTop, rmseValueTop, wpsnrValueTop, ...
    'VariableNames', {'PSNR', 'Euclidean_Error', 'MSE', 'RMSE', 'WPSNR'});

tableValuesBottom = table(psnrValueBottom, euclideanErrorBottom, mseValueBottom, rmseValueBottom, wpsnrValueBottom, ...
    'VariableNames', {'PSNR', 'Euclidean_Error', 'MSE', 'RMSE', 'WPSNR'});

% Display the tables
disp('Image Quality Metrics for Top 4 Bits:');
disp(tableValuesTop);
disp('Image Quality Metrics for Bottom 4 Bits:');
disp(tableValuesBottom);

% Calculate SSIM for the top and bottom combined images
ssimTop = ssim(grayscaleImage, newImageTop);
ssimBottom = ssim(grayscaleImage, newImageBottom);

% Display SSIM values
fprintf('SSIM for Top 4 Combined Bits: %.4f\n', ssimTop);
fprintf('SSIM for Bottom 4 Combined Bits: %.4f\n', ssimBottom);

% Construct a similarity graph
similarityGraph = [1, ssimTop; 2, ssimBottom];

% Display the similarity graph
figure;
bar(similarityGraph(:, 2));
xticks(1:2);
xticklabels({'Top 4 Combined Bits', 'Bottom 4 Combined Bits'});
ylabel('SSIM Value');
title('Similarity Graph');
