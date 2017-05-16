# Creating test for CNN object detection component

21 Jun 2016

# Design Specifications:

This components has following tasks: firstly, read an image from command line argument, secondly pass the image to CNNobjectdetection component in raw RGB format, thirdly collect the detection results and finally display them on image.

# CSDL interface file for the component:


```
import "/robocomp/interfaces/IDSLs/objectDetection.idsl";

Component testObjectDetectionComp
{
	Communications
	{
		requires objectDetection;
	};
	language Cpp;
	gui Qt(QWidget);
};

```


# Specific worker compute method:

1.  run once: read image file from argument and pass to objectdetectionCNN module.

    Mat src=imread(inputFile,1);

2.  convert to ColorSeq format (raw RGB)

    int k=0; ColorSeq rgbMatrix; rgbMatrix.resize(src.rows*src.cols); for(int i=0;i<=src.rows-1;i++) { for(int j=0;j<=src.cols-1;j++) { Vec3b intensity = src.at<vec3b>(i,j); rgbMatrix[k].blue = intensity.val[0]; rgbMatrix[k].green = intensity.val[1]; rgbMatrix[k].red = intensity.val[2]; k++; } }</vec3b>

3.  call objectdetectioncnn module and get detections.

objectdetectioncnn_proxy->getLabelsFromImage(rgbMatrix, src.rows, src.cols, result);

1.  Display detection Bounding boxes.


    ```
         for(unsigned int i=0; i < result.size();i++ )
             {  <<result[i].bb.width<<" "<<result[i].bb.height<<endl;
                 Rect r1;
                 BoundingBox bb;
                 bb=result[i].bb;
                 r1.x=bb.x;
     				r1.y=bb.y;
     				r1.width=bb.width;
     				r1.height=bb.height;
     				Scalar rect_color = Scalar( 0, 0, 255 );
                 rectangle( src, r1.tl(), r1.br(), rect_color, 2, 8, 0 );
                 string name=result[i].name;
                 putText(src, name.substr(0, name.find(",")), r1.tl(), CV_FONT_HERSHEY_DUPLEX, 0.5, cvScalar(0,0,250), 1, CV_AA);
             }              
     	imshow("Source image", src ); Results: ============     <img src="images/week5/week5_Real1.png" alt="" width="400"/>  <img src="images/week5/week5_Real2.png" alt="" width="400"/>  <img src="images/week5/week5_Real3.png" alt="" width="400"/> 

    ```
