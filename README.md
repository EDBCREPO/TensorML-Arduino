# Neural-Network-C
This is a simple c++ implementation of different image processing and IA algorithms that can be compiled on a microcontroller such as Arduino uno or an Esp32. 

NOTE: this code was specially designed to be implemented in a smart control prototype, which was designed to detect objects in a controlled environment.

# Algorithms

### Algebra Algorithms
```
#include "stdio.h"
#include "stdlib.h"
#include "../Libraries/PC/tensor.h"

//Define Tensors
tensor A,B,C,D,E,F;

//Predefine Tensor B Values
float B_ARRAY[9] = {
	5,10,20,
	2, 4, 6,
	3, 6, 9,
}; 

int main(){

	//Initialize Tensors
	A.relloc(3,3); //3x3 Matrix
	B.relloc(3,3); //3x3 Matrix
	
	//Set Tensor Values
	B.set_array(B_ARRAY);
	A.set_one();
	
	//Show Tensors
	puts("Tensor A:"); A.show_tensor();
	puts("Tensor B:"); B.show_tensor();

	
	C = A + B; C.show_tensor(); //Add Tensors
	
	D = A - B; D.show_tensor(); //Substract Tensors
	
	E = A * B; E.show_tensor(); //Multiply Tensors
	
	//Tensor and Scalar Operations
	F = B * 10; F.show_tensor();	
	
	//Delete Tensor
	B.delloc(); 
	A.delloc();
	
	return 0;}

```

### Image Algorithms

- resize
```
	tensor img = decrease_image(img2tensor("Path/To/Image"),100);
```

- Paddind
```
	tensor img = padding(img2tensor("Path/To/Image"),_padding_size);
```

- Maxpooling
```
	tensor img = maxpooling(img2tensor("Path/To/Image"));
```

- Convolution
```
	tensor filter = get_filter( sobel_edge );
	tensor img = convolution(img2tensor("Path/To/Image"),_filter);
```

- Correlation
```
	tensor filter = get_filter( sobel_edge );
	tensor img = correlation(img2tensor("Path/To/Image"),_filter);
```

- Thresholding
```
	tensor img = thresholding(img2tensor("Path/To/Image"),_thresholding_value);
```

- Difference Edge Detection
```
	tensor DED = difference_edge_detection(img2tensor("Path/To/Image"));	
```

- Local Binary Pattern
```
	tensor lbp = local_binary_pattern(img2tensor("Path/To/Image"));
```

- haar filters
```
	tensor image_base = img2tensor("Path/To/Image");
	tensor integral_image( image_base );
	
	tensor img( image_base.size[0],image_base.size[1] );

	//Vertical Edge Haar Detector 1x2
	for(int i=img.size[1]-(sy*2); i--;){ for(int j=img.size[0]-sx; j--;){
		img.data[img.get_index(i,j)] = haar_filter_edge_ver( integral_image,i,j,sy,sx );
	}}

	//Horizontal Edge Haar Detector 2x1
	for(int i=img.size[1]-sy; i--;){ for(int j=img.size[0]-(sx*2); j--;){
		img.data[img.get_index(i,j)] = haar_filter_edge_hor( integral_image,i,j,sy,sx );
	}}

	//Vertical Line Haar Detector 1x3
	for(int i=img.size[1]-(sy*3); i--;){ for(int j=img.size[0]-sx; j--;){
		img.data[img.get_index(i,j)] = haar_filter_line_ver( integral_image,i,j,sy,sx );
	}}

	//Horizontal Line Haar Detector 3x1
	for(int i=img.size[1]-sy; i--;){ for(int j=img.size[0]-(sx*3); j--;){
		img.data[img.get_index(i,j)] = haar_filter_line_hor( integral_image,i,j,sy,sx );
	}}

	//Outline Haar Detector 2x2
	for(int i=img.size[1]-(sy*2); i--;){ for(int j=img.size[0]-(sx*2); j--;){
		img.data[img.get_index(i,j)] = haar_filter_outline( integral_image,i,j,sy,sx );
	}}
	
	
	img = activation(img,sigmoid,true);
```

- Template Matching
```
	tensor img_base= decrease_image(img2tensor("Dataset/vr/2.jpg"),100);
	tensor img_template= img2tensor("Dataset/vr/base.jpg");
	tensor label = template_matching( img_base,img_template,0.70);
```


### IA Algorithms

- Multi Layer Neural Network
```
	mlp M( 3,0.3,sgd,cross_entropy ); //Neural Network Topology
//	mlp M( a,b,c,d );
		//a = Number of Layers
		//b = learning rate
		//c = Optimization
		//D = cost Function
	
	M.add_layer(4,sigmoid,input_layer);	//input Layer
	M.add_layer(5,sigmoid,hidden_layer);//hidden Layer
	M.add_layer(3,softmax,output_layer);//output Layer
	
	M.compile();
	
	M.fit( _Input_array, _output_array ); //fit the neural network
	M.predict( Test_input_array ); //Make a prediction
```

- Recurrent Neural Network
```
	rnn M(3,0.03,momentum,mean_squared); //Neural Network Topology
//	rnn M( a,b,c );
		//a = Number of Layers > 3
		//b = learning rate
		//c = Optimization
		//D = cost Function
	
	M.add_layer(4,tangh,input_layer);
	M.add_layer(5,tangh,hidden_layer);
	M.add_layer(3,softmax,output_layer);
	M.compile();
	
	M.fit( _Input_array, _output_array ); //fit the neural network
	M.predict( Test_input_array ); //Make a prediction
```

- Radial Basis Function Neural Network
```
	rbfnn M(0.03,momentum,mean_squared); //Neural Network Topology
//	rbfnn M( a,b,c );
		//a = learning rate
		//b = Optimization
		//c = cost Function
		
	M.add_layer(6,softplus,input_layer); //input Layer
	M.add_layer(6,softplus,hidden_layer);//hidden Layer
	M.add_layer(1,linear,output_layer);  //outpu Layer
	M.compile();

	M.layer[0].Array[1].set_array( _Random_Input_Array_ ); //Init Bias
	
	M.fit( _Input_array, _output_array ); //fit the neural network
	M.predict( Test_input_array ); //Make a prediction
```

# Images

simple face detection

![Captura de pantalla de 2020-06-22 11-25-00](https://user-images.githubusercontent.com/41095555/156239925-b92245c7-121b-4c1f-8b74-9ed04b40fa66.png)

![Captura de pantalla de 2020-06-21 12-39-18](https://user-images.githubusercontent.com/41095555/156247090-3dc8836a-0c5a-4779-b1f7-333d36ab5b29.png)


# Compile
```bash
# to Compile a CV example, you should have to write this:
g++ -std=c++11 -I /usr/include/opencv -I/usr/include/opencv2 -L/usr/lib/ "EXAMPLE/PATH.CPP" -o main -lopencv_core -lopencv_highgui -lopencv_video -lX11 -pthread

# if you want to compile a simple IA algorith, you should have to write this:
g++ -std=c++11 "EXAMPLE/PATH.CPP" -o main
```
