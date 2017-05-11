<div class="content container">_**RoboComp** is an open-source Robotics framework providing the tools to create and modify software components that communicate through public interfaces. Components may require, subscribe, implement or publish interfaces in a seamless way. Building new components is done using two domain specific languages, IDSL and CDSL. With IDSL you define an interface and with CDSL you specify how the component will communicate with the world. With this information, a code generator creates C++ and/or Python sources, based on CMake, that compile and execute flawlessly. When some of these features have to be changed, the component can be easily regenerated and all the user specific code is preserved thanks to a simple inheritance mechanism._

* * *

<div class="posts">

<div class="post">

# [Top-Hat filter based object proposal generation](/website/2016/06/15/HaritWeek4/)

<span class="post-date">15 Jun 2016</span>

# Top-Hat filter based object proposal generation:

As the classical CNNs like Alexnet, Caffenet, VGG16 give a classification oputput. A method is required to find object proposals in the scene and give it to CNN for classification. I have used bilateral and top-hat filter to generate object proposals. The code uses filter implementations in openCV.

The pipeline is as follows:

1.  Load an image

    src = imread( file );

2.  Bilateral Filtering: edge preserving color and spatial filter.

    bilateralFilter(src, blt, 8, 40, 5, 1 );

3.  Apply the specified morphology operation (tophat).

<div class="highlighter-rouge">

```
int operation = 4 + 2;

```

</div>

Mat element = getStructuringElement( morph_elem, Size( … 2_morph_size + 1, 2_morph_size+1 ), Point( morph_size, morph_size ) ); morphologyEx( blt, tophat, operation, element);

1.  Thresholding to create binay image.

    cvtColor(tophat, gray, CV_BGR2GRAY); bw = threshval < 128 ? (gray > threshval) : (gray < threshval);

2.  Labeling each connected segment.

    int nLabels = connectedComponents(bw, labelImage, 8);

3.  Fitting bounding-box to segments.

* * *

Harit Pandya

</div>

<div class="post">

# [Caffe C++ API](/website/2016/06/11/HaritWeek3/)

<span class="post-date">11 Jun 2016</span>

# Working with Caffe CPP API:

This tutorial explains the Caffe classification example. Implementations are present in <caffe_root>/examples/cpp_classification.</caffe_root>

Required inputs:

1.  Model file: These is the file describing the layer configuration and its hyperparameters.
2.  trained_file: These is the file containing learned weights.
3.  mean_file: It is good idea to subtract the mean of all images in dataset. It essentially does background subtraction.
4.  label_file: CNN gives prediction probability for all the categories on which it was trained. The label file gives mapping between category id and class name.

Pipeline:

1.  set device(CPU/GPU)
2.  Load the network net_.reset(new Net<float>(model_file, TEST)); net_->CopyTrainedLayersFrom(trained_file);</float>
3.  Load the binaryproto mean file. SetMean(mean_file);
4.  Set input and output layer (Note: only single input and output layer supported.)
    Blob<float>* input_layer = net_->input_blobs()[0]; Blob<float>* output_layer = net_->output_blobs()[0];</float></float>
5.  Compute prediction for given test image. std::vector <prediction>predictions = classifier.Classify(image); The prediction structure has two components. Prediction.first gives label and Prediction.second gives corresponding score.</prediction>

**The prediction method pipeline is given as follows:**

1.  Change the dimension of input layer

    input_layer->Reshape(1, num_channels_, input_geometry_.height, input_geometry_.width);

2.  Forward dimension change to all layers.

<div class="highlighter-rouge">

```
net_->Reshape();

```

</div>

1.  Wrap the input layer of the network in separate cv::Mat objects (one per channel). This way we save one memcpy operation and ww don’t need to rely on cudaMemcpy2D. The last preprocessing operation will write the separate channels directly to the input layer.

<div class="highlighter-rouge">

```
WrapInputLayer(&input_channels);

```

</div>

1.  Convert the input image to the input image format of the network and write the separate BGR planes directly to the input layer of the network.

    Preprocess(img, &input_channels);

2.  Run forward pass for the network.

    net_->ForwardPrefilled();

3.  Copy the output layer to a std::vector

    Blob<float>* output_layer = net_->output_blobs()[0]; const float* begin = output_layer->cpu_data(); const float* end = begin + output_layer->channels(); return std::vector<float>(begin, end);</float></float>

* * *

Harit Pandya

</div>

<div class="post">

# [Progress -- Automatic the uploading of binary files using git-annex](/website/2016/06/11/swapsharmaPost2/)

<span class="post-date">11 Jun 2016</span>

This is the second post in the series of post pertaining to the project “Automatic the uploading of binary files using git-annex”. This post aims to show the progress made until the mid term evaluation.

We planned to write abstraction over git-annex to add the files to remote storage server. Git-annex provides support for external special remote, it follows a messaging based service as prescribed here [External Special Remote Protocol](https://git-annex.branchable.com/design/external_special_remote_protocol/) needs to be followed.

But we found some tools [git-annex-remote-rclone](https://github.com/DanielDent/git-annex-remote-rclone), [git-annex-remote-hubic](https://github.com/Schnouki/git-annex-remote-hubic), [dropboxannex](https://github.com/TobiasTheViking/dropboxannex), which were already present , so we decided to test them, debug them and change them according to our requirement. we tried to install and test all of them, and all of them were stuck with some or the other kind of error.

In the case of droppboxannex, git init was taking a lot of time, opened issue. Similarly, for the other two as well ther some errors. We decided to focus our energy on one of them and found that git-annex-remote-rclone was the best out of all of them, as it can support all the remote storage spaces which are supported by rclone, which includes a lot of them from dropbox, hubic to Yandex Disk. So we decided to debug it.

It was showing an error `"Remote origin not usable by git-annex; setting annex-ignore"`. We tried to pinpoint the error, but it looked like the error was occuring out of git-annex, somehow git-anex was not able to use remote rclone, after a lot of struggling, it came to our notice that the message `DIRHASH-LOWER Key` was `First supported by git-annex version 6.20160511` and since we were using the official version supported by ubunutu version. So after cloning the git-annex repo and builiding and installing from the source we were able to run the git-annex-remote-rclone.

Now the tool works and all, but as already present in the problem statment, we need to build a tool which can further ease the process, so if we can create a little bit of more abstraction it will work best. So we decided to built a little bit of more abstraction according to following steps:

<div class="highlighter-rouge">

```
Installing rlcone [Once only]
1\. Install rclone into your $PATH, e.g. /usr/local/bin
2\. Copy git-annex-remote-rclone into your $PATH
3\. Configure an rclone remote: rclone config

Using git annex remote
1\. git annex init
2\. git annex initremote <remote_name> type=external externaltype=rclone target=<rclone_target_name> prefix=git-annex chunk=50MiB encryption=shared mac=HMACSHA512 rclone_layout=lower
3\. git annex add <binary_files>
4\. git commit -am <message> <binary_files>
5\. git push -u origin master
6\. git annex sync --content
7\. git annex copy <binary_files> --to <remote_name>

Cloning repos
1\. git annex clone <repo address>
2\. cd <cloned_repo_folder_name>
3\. git annex sync
4\. git annex enableremote <remote_name>
5\. git annex get <binary_files> --from <remote_name>

```

</div>

The abstraction over the above steps will be as follows:

<div class="highlighter-rouge">

```
Install Robocomp

Installing rclone [once]
1\. rclone config

Using abstract tool
1\. git_tool initremote <remote_name> type=external externaltype=rclone target=<rclone_target_name> prefix=git-annex chunk=50MiB encryption=shared mac=HMACSHA512 rclone_layout=lower
2\. git_tool add <binary_files> <remote_name>

Cloning repos
1\. git annex clone <repo address>
2\. cd <cloned_repo_folder_name>
3\. git_tool get <binary_files> <remote_name>

```

</div>

Swapnil Sharma

</div>

<div class="post">

# [week 3 updates](/website/2016/06/05/yashWeek3/)

<span class="post-date">05 Jun 2016</span>

# Basic coding

Now that i am decided about the different features about the rcmaster. Pablo suggested about using something similer to how ros 2.0 is implementing node discovery. They are planning to use an external middle-ware named DDS. DDS uses an custom lightweight discovery protocol for finding other nodes. It eliminates the need for an central node keeping registry of all nodes. But after some discussion with other community members we decided to stick with rcmaster as using DDS will negate robocomp’s middle-ware independent policy and also it doesn’t give much benefits compared to the complexity in implementation.

Also we discussed about different ways to implement the multi robot scenario. Multi Robot scenarios can be implemented in 2 ways. one, we could have only one robot running rcmaster and all other robots will be configured to use this rcmaster. in This case the whole load will be into this rcmaster. This doesn’t require any extra coding in the rcmaster. we will only need to change the environment variable which points to the rcmaster in all robots. But the downside is there may be heavy latency as there is only one rcmaster and all the lookups and registrations are RPC’s. Other downside is that if that one Robot which is hosting the rcmaster fails or gets disconnected, then all other robots will fail. The 2nd solution is to let all robots have local rcmasters and they will need to sync their registry with all other robots in th network. But in this case the issue is how will the robots find each other. One straight forward solution is to hard-code all the robots ip in each of the robots. But this may be really tedious. Hence we may use some discovery protocol like udp multicast for finding other rcmasters.

Also this week i wrote a basic interface for the rcmaster. Both the idsl and and the slice file. and had a discussion about them with community members. After making a few changes suggested. I will begin with rest of implementation.

* * *

Yash Sanap

</div>

<div class="post">

# [Object detection using caffe](/website/2016/06/04/HaritWeek2/)

<span class="post-date">04 Jun 2016</span>

# Is CNN solution to every object classification problem?

Given the hype about CNNs and how they outperform the state-of-art object classifiers it would be interesting to see how they perform on images of day-today objects placed o a table . Figure 1 shows sample images given to CNN for classification. Although being a simple non-cluttered object identification problem, it is a non-trivial and challenging for vision tasks. Because, the the resolution is low and and objects are small. CNNs are good classifiers for a primary object in the scene i.e. we need to extract the object before giving it to a CNN. So let’s manually crop every object and give it to CNN and see how it performs. I have used 2 state-of-art CNNs. One is VGG16 by Oxford research group and the other is Overfeat by CILVR Lab @ NYU. I tried to qualitatively analyse the performance of both networks using top and top-5 classification results. I found that the performance is not very good for top-result, however the performance is acceptable for top-5 results.

![alt tag](images/week1/week1_dataset.png)

Figure 1: Settings we are interested to work with. A table with some objects.

# How to handle Object detections?

Object detection task is different from classification task. In detection task it is required to identify where the object is in the given image compared to the classification task where given the image identification of the object’s category is required. Recently, object detection has gained lots of interest in the vision community. At present there exists NN based solutions like Segnets, FCN which do pixelwise segmentations, also there is RCNN which proposes bounding box for an object. Now several such bounding boxes are given to the classifier for category identification. Here I have used a top-hat filter for object detection and compared with RCNN. The basic idea is that for objects there is intensity difference from the table so that the object could be discriminated from table by a top-hat filter. From figure 2\. It could be seen that top-hat gives better object proposals than RCNN. Note that I have used existing pre-trained networks thus the cliam will not hold if I retrain RCNN on our dataset. So we indeed require training.

![](images/week1/week1_rcnn_71.jpg) ![](images/week1/week1_rcnn_192.jpg) ![](images/week1/week1_rcnn_445.jpg) ![](images/week1/week1_rcnn_706.jpg) ![](images/week1/week1_overfeat_71.jpg) ![](images/week1/week1_overfeat_192.jpg) ![](images/week1/week1_overfeat_445.jpg) ![](images/week1/week1_overfeat_706.jpg) ![](images/week1/week1_VGG16_71.jpg) ![](images/week1/week1_VGG16_192.jpg) ![](images/week1/week1_VGG16_445.jpg) ![](images/week1/week1_VGG16_706.jpg)

Figure 2: Object detection results form RCNN (row 1) compared to my object proposel fed to Overfeat (row 2) and VGG16 (row3) for sample images.(Kindly click on images to view in larger size.)

# Next steps

Next I will use RCNN and train on our dataset or another approach is to use better object proposal algorithms like BING, GOP, Selective search (RCNN uses selective search) and evaluate the performance.

* * *

Harit Pandya

</div>

</div>

<div class="pagination">[Older](/webrobocomp.github.io/page5) [Newer](/webrobocomp.github.io/page3)</div>

</div>