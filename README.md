# Image-Steganography
This is a small scale implementation of image steganography using Signal Processing techniques. The technique used here is to apply discrete wavelet transform to obtain the wavelet coefficient matrices and performing singular value decomposition over them to embed the details of the data image in the cover image. The encoding wavelet used here is “db8” of the Daubechies wavelet for its ability to capture both high-frequency and low-frequency details efficiently, and for analysis across different frequency bands while maintaining a relatively good level of compression.

The dataset is stored in a json file which holds details such as the list of names of the patients, their cover photo, their bio medical detail photo such as their CT-Scan or MRI Scan and the respective passwords for the decoding process to be enabled. Any other data holding files such as csv can also be used. The dataset is loaded into the main file and converted from BGR to RGB formats, and the files are prepared for encoding process.

**A. Mathematical Prelimanaries**

_1) Singular Value Decomposition_

• The SVD of any rectangular matrix produces three unitary matrices U, V and S. U and V are having singular values whereas S singular values are diagonal.

•	For example, a 3×3 matrix and its SVD decomposition are given below. It is visible that matrix A, U, V contains 9 elements but matrix S can be represented by principle diagonal elements because non-diagonal elements are zero.

![image](https://github.com/RanjithBMR/Image-Steganography/assets/147130369/2ae3c18c-998e-45f1-8fca-6147adb2c7d9)



• So, instead of sending a complete message image the proposed technique sending only the S matrix and complete message image could reconstruct using USV matrices. Furthermore, when the image is somewhat disrupted, the diagonal elements of the singular value matrix acquired from singularvalue decomposition do not vary appreciably, indicating that the singular-value decomposition of the matrix is rotation invariant. In this regard, adding the secret image after the image's singularvalue decomposition can boost the algorithm's anti-attack performance.

_2) Discrete Wavelet Transform_
• DWT is a very popular mathematical tool in the field of signal analysis to decompose any image in low (LL) and high (HH) components.

• The Low-frequency component contains more information and the high component contains less information. So, any change in a cover image due to data hiding in the HH component causes less distortion in the cover image. HH component of DWT matrix will hold the secret data.

• DWT has several advantages, including the ability to withstand multiple attacks while maintaining the image's original quality and ensuring the integrity of secret information collected.

• The following image shows DWT Algorithm

![image](https://github.com/RanjithBMR/Image-Steganography/assets/147130369/ffd0336e-b432-4916-b41c-022e3cb5ddea)



**B. Encoding Process**

_1) Splitting the Red, Green and Blue channels_

• The three main channels of the images are split and stored in three different arrays which are two dimensional.

• The respective arrays contain pixel values representing the respective colour intensities of each pixel in the image. Each value in this array denotes the amount of red colour present at the corresponding pixel location.

• This is done for both the cover and data to be hidden images.

_2) Apply DWT to the obtained channels_

• DWT is applied to the respective red, green and blue channel matrices and the corresponding coefficients for approximation, horizontal and vertical details are obtained.

• This is done for both the cover and data to be hidden images.

_3) Perform SVD over the DWT coefficient matrices_

• SVD is applied over the respective red, green, and blue channel DWT coefficient matrices to obtain the corresponding left singular vectors matrix, the singular values array and right singular vectors matrix.

• The vectors form an orthonormal basis for column space of original matrix which represent a direction in the input space, a singular value 1-D array that describes the 
"importance" or "strength" of each corresponding singular vector and a right singular value matrix with vectors that form an orthonormal basis for the row space of the original matrix which corresponds to a direction in the output space, respectively.

• This is done for both the cover and data to be hidden images.

_4) Embed the information and reconstruct the matrix_

• Embed the information of the data image to the cover image from the singular value array by multiplying with a scaling factor which determines how much of the image is to be embedded. A higher scaling factor would embed more information, but image quality will be lost.

• Apply these for all three channels (red, green and blue)

• Reconstruct the coefficient matrix from the embedded SVD parameters by scalar matrix multiplication.

• Perform type casting on the reconstructed coefficients for each colour channel convert the data type of each element in the matrix from its current type to integers.

• Merge the three channels to form a single image matrix.

_5) Extract details and apply IDWT to generate steganographic image_

• Split the processed image into its individual colour channels and pair each channel with its corresponding detail coefficients and store it in a tuple.

• The resulting tuple is taken, and inverse DWT is performed. It reconstructs the image data from the wavelet coefficients, thereby reversing the process of wavelet decomposition.

• The three reconstructed colour channels are merged into a single multi-channel image matrix which is the final steganographic image.

Flowchart for the encoding process is as follows:

<img width="296" alt="image" src="https://github.com/RanjithBMR/Image-Steganography/assets/147130369/58c7969d-3068-49a1-8976-7f7ee101cd1b">



**C. Decoding Process**

_1) Check if the password entered by user for the patient and proceed to decode only if the password is correct_

_2) Apply DWT on the stego image_

• Apply DWT as in encoding to obtain the approximate, horizontal, vertical and diagonal coefficient details for all three channels (red, green and blue)

_3) Perform SVD over the coefficient matrices_

• Obtain the left singular value matrices, singular value 1-D array and right singular value matrices.

_4) Reverse the information embedded in the 'D' parameter of the cover image that was performed in encoding process_

_5) Combine the approximations with the hidden SVD values to reconstruct the hidden image_

_6) Pair each colour channel's pixel values with its respective DWT coefficients in a tuple to perform IDWT_

_7) Apply inverse transform to each channel of the image to generate the final hidden information image_

• Hidden stego images get high resolution R,G,B separate images/channels using IDWT

_8) Combine the different high resolution r,g,b to get the final hidden information_

Flowchart for the decoding process is as follows:

![image](https://github.com/RanjithBMR/Image-Steganography/assets/147130369/6aeadad4-dc6c-4cc6-8f59-4de8232e0d8f)
