# Underwater-Image-Quality-project
% Two-Step Enhancement Framework for Improving Underwater Image Quality
% Step 1: Color Correction
% Step 2: Contrast Enhancement

clc;
clear;
close all;

% Load the underwater image
inputImage = imread('underwater_image.jpg'); % Replace with your image path
figure;
imshow(inputImage);
title('Original Underwater Image');

% Step 1: Color Correction
% Apply White Balance to correct color cast
colorCorrectedImage = whiteBalance(inputImage);
figure;
imshow(colorCorrectedImage);
title('Color Corrected Image');

% Step 2: Contrast Enhancement
% Apply Contrast-Limited Adaptive Histogram Equalization (CLAHE)
contrastEnhancedImage = contrastEnhancement(colorCorrectedImage);
figure;
imshow(contrastEnhancedImage);
title('Contrast Enhanced Image');

% Save the final enhanced image
imwrite(contrastEnhancedImage, 'enhanced_underwater_image.jpg');

% Function for White Balance (Color Correction)
function correctedImage = whiteBalance(image)
    % Convert the image to double for calculations
    image = double(image);
    
    % Calculate the mean of each color channel
    meanR = mean(mean(image(:,:,1)));
    meanG = mean(mean(image(:,:,2)));
    meanB = mean(mean(image(:,:,3)));
    
    % Compute the scaling factors
    scaleR = meanG / meanR;
    scaleB = meanG / meanB;
    
    % Apply the scaling factors to each channel
    correctedImage(:,:,1) = image(:,:,1) * scaleR;
    correctedImage(:,:,2) = image(:,:,2);
    correctedImage(:,:,3) = image(:,:,3) * scaleB;
    
    % Normalize the image to the range [0, 255]
    correctedImage = uint8(correctedImage);
end

% Function for Contrast Enhancement (CLAHE)
function enhancedImage = contrastEnhancement(image)
    % Convert the image to LAB color space
    labImage = rgb2lab(image);
    
    % Apply CLAHE to the L channel (lightness)
    labImage(:,:,1) = adapthisteq(labImage(:,:,1), 'ClipLimit', 0.02, 'Distribution', 'rayleigh');
    
    % Convert the image back to RGB color space
    enhancedImage = lab2rgb(labImage);
    enhancedImage = uint8(enhancedImage * 255);
end
