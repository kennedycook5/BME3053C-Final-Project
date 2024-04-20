# BME3053C-Final-Project
Final Project on Meningioma Tumor Detection in transverse MRI scans. Developed by Amelia Adin, Hallie Bardani, Kennedy Cook, and Erika Malits.

clc; clear;
Img = input('Enter image file name: ', 's');
img = imread(Img);
subplot(2,3,1);
imshow(img);
title('MRI of Patient');
binary_img = img > 160
gray_img = uint8(binary_img)*255;
subplot(2,3,2);
imshow(gray_img);
title('MRI Binary Image');
gray_img_double = double(im2gray(gray_img));
B = imerode(gray_img_double,ones(19));
subplot(2,3,3);
imshow(B);
title('Eroded Thin Shapes');
C = imdilate(B,ones(19));
subplot(2,3,4);
imshow(C);
title('Dilated to Original Size');
BWfill = imfill(C,'holes');
BWdil = imdilate(BWfill,ones(2));
subplot(2,3,5);
imshow(BWdil)
title('Filled Remaining Holes');
subplot(2,3,6)
imshow(labeloverlay(gray_img,BWdil));
title('Highlighted Tumor on MRI');
[rows, columns] = size(BWdil);
if bwarea(BWdil(:, :)) == 0
    fprintf('No tumor was found for this patient.');
elseif bwarea(BWdil(:, 1:floor(columns/2))) == 0 && bwarea(BWdil(1:floor(2*rows/3))) == 0
    fprintf('\nThis patient has a posterior right-sided meningioma. \n\nThe patient may be experiencing the following symptoms: \n- difficulty walking \n- loss of balance\n- vertigo\n- nausea\n- loss of smell\n- headaches\n- changes in vision\n- hearing loss\n- seizures\n- changes in thought\n\nSymptoms will vary depending on how far the tumor extends into the brain.')
elseif bwarea(BWdil(:, floor(columns/2)+1:columns)) == 0 && bwarea(BWdil(1:floor(2*rows/3))) == 0
    fprintf('\nThis patient has a posterior left-sided meningioma. \n\nThe patient may be experiencing the following symptoms:\n- difficulty walking \n- loss of balance\n- vertigo\n- nausea\n- loss of smell\n- headaches\n- changes in vision\n- hearing loss\n- seizures\n- changes in thought\n\nSymptoms will vary depending on how far the tumor extends into the brain.')
elseif bwarea(BWdil(:, 1:floor(columns/2))) > bwarea(BWdil(:, floor(columns/2)+1:columns)) && bwarea(BWdil(1:floor(2*rows/3))) == 0
    fprintf('\nThe tumor is likely a posterior parasagittal meningioma, with most of the mass on the left side of the brain. \n\nThe patient may be experiencing the following symptoms: \n- tremors\n- seizures\n- headaches\n- personality changes\n- dementia\n- unsteadiness\n- difficulty walking\n- loss of balance\n- vertigo\n-  nausea\n- loss of smell\n- headaches\n- changes in vision\n- hearing loss\n- seizures\n- changes in thought\n\nSymptoms will vary depending on how far the tumor extends into the brain.')
elseif bwarea(BWdil(:, 1:floor(columns/2))) < bwarea(BWdil(:, floor(columns/2)+1:columns)) && bwarea(BWdil(1:floor(2*rows/3))) == 0
    fprintf('\nThe tumor is likely a posterior parasagittal meningioma, with most of the mass on the right side of the brain. \n\nThe patient may be experiencing the following symptoms: \n- tremors\n- seizures\n- headaches\n- personality changes\n- dementia\n- unsteadiness\n- difficulty walking\n- loss of balance\n- vertigo\n-  nausea\n- loss of smell\n- headaches\n- changes in vision\n- hearing loss\n- seizures\n- changes in thought\n\nSymptoms will vary depending on how far the tumor extends into the brain.')
elseif bwarea(BWdil(:, 1:floor(columns/2))) == 0 
    fprintf(‘\nThis patient’s meningioma is located on the right side of the brain. \n\nThe patient may be experiencing the following symptoms: \n- loss of smell\n- headaches, changes in vision\n- hearing loss\n- seizures\n- changes in thought \n\nSymptoms may vary depending on how far the tumor extends into the brain.');
elseif bwarea(BWdil(:, floor(columns/2)+1:columns)) == 0
    fprintf('\nThis patient’s meningioma is located on the left side of the brain. \n\nThe patient may be experiencing the following symptoms: \n- loss of smell\n- headaches, changes in vision\n- hearing loss\n- seizures\n- changes in thought \n\nSymptoms may vary depending on how far the tumor extends into the brain.');
elseif bwarea(BWdil(:, 1:floor(columns/2))) > bwarea(BWdil(:, floor(columns/2)+1:columns))
    fprintf('\nThe tumor is likely a falx or parasagittal meningioma, with most of the mass on the left side of the brain. \n\nThe patient may be experiencing the following symptoms: \n- tremors\n- seizures\n- headaches\n- personality changes\n- dementia\n- unsteadiness ');
elseif bwarea(BWdil(:, 1:floor(columns/2))) < bwarea(BWdil(:, floor(columns/2)+1:columns))
    fprintf('\nThe tumor is likely a falx or parasagittal meningioma, with most of the mass on the right side of the brain. \n\nThe patient may be experiencing the following symptoms: \n- tremors\n- seizures\n- headaches\n- personality changes\n- dementia\n- unsteadiness ');
end


