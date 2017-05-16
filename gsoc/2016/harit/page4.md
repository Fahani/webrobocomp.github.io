# Top-Hat filter based object proposal generation

15 Jun 2016

# Top-Hat filter based object proposal generation:

As the classical CNNs like Alexnet, Caffenet, VGG16 give a classification oputput. A method is required to find object proposals in the scene and give it to CNN for classification. I have used bilateral and top-hat filter to generate object proposals. The code uses filter implementations in openCV.

The pipeline is as follows:

1.  Load an image

    src = imread( file );

2.  Bilateral Filtering: edge preserving color and spatial filter.

    bilateralFilter(src, blt, 8, 40, 5, 1 );

3.  Apply the specified morphology operation (tophat).


```
int operation = 4 + 2;

```


Mat element = getStructuringElement( morph_elem, Size( … 2_morph_size + 1, 2_morph_size+1 ), Point( morph_size, morph_size ) ); morphologyEx( blt, tophat, operation, element);

1.  Thresholding to create binay image.

    cvtColor(tophat, gray, CV_BGR2GRAY); bw = threshval < 128 ? (gray > threshval) : (gray < threshval);

2.  Labeling each connected segment.

    int nLabels = connectedComponents(bw, labelImage, 8);

3.  Fitting bounding-box to segments.

* * *

Harit Pandya