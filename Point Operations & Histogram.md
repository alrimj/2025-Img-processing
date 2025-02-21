# Point Operations
`point operations` perform a mapping of the pixel values without chainging the size, geometry or local structure of the image.
Each new pixel value  I'(u,v) depends on the previous value  I(u,v) at the same position and on a mapping function f(.)
the function f() is independent of th ecoordinates.
such operation is called "homogeneous"

Example of homogeneous point operations : 
- Modifying image brightness or contrast
- Applying arbitrary intensity transformation (curves)
- Quantizing (posterizing) images
- Global thresholding
- Gamma correction
- Color transformations

A nonhomogeneous point operation g() would also take into account the current image coordinate (u,v)

- changing contrast and brightness
  - f contr(a) = a X 1.5
  - f bright(a) = a + 10
- limiting results by Clamping
  - if (a > 255) a = 255;;
  - if (a < 0) a = 0;
  - ```
    public void run(ImageProcessor ip) {
      int w = ip.getWidth();
      int h = ip.getHeight();

      for (int v = 0; v < h; v++){
        for (int u = 0; u < w: u++){
          int a = (int) (ip.get(u, v) * 1.5 + 0.5);
          if (a > 255)
            a = 255;  //clamp to maximum value
          ip.set(u, v, a);
          }
        }
      }
    ```
- Inverting images
  - f invert(a) = -a + a.max = a.max - a
   
- Threshold operation
  - Thresholding an image is a special type of quantization that separates the pixel values in 2 classes, depending on a given threshold valye a.th
  - the threshold function maps all the pixels to one of two fixed intensity value a.0, a.1
  - f threshold(a) = a.0 for a < a.th or a.1 for a >= a.th (0 < a.th <= a.max)
    - ex_ binarization: a.0 = 0, a.1 = 1

## Point Operations and Histograms
the effect of some point operations on histograms are easy to predict.
ex) increasing the brightness, raising the entries
point operations can only shift and merge histogram entries.
operations that result in merging histogram bins are irreversible!

## automatic contrast Adjustment
Auto-contrast : a point operation that modifies the pixels such that the available range of values is fully covered.
Linear stretching of the intensity range can result in gaps in the new histogram.

## Better Auto-contrast
Its better to map only certain range of the values and get rid of the tails(usually noise) based on predefined percentiles(S.low,S.high)

# Histogram

## Histogram Equalization
Adjust two different images in such a way that their
resulting intensity distribution are similar
Useful when comparing images to get rid of illumination
variations
The goal is to find and apply a point operation such that
the histogram of the modified image approximates a
uniform distribution. 
