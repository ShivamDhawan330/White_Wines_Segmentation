# **White Wines Segmentation**

https://huggingface.co/spaces/ShivamDhawan330/White_Wines_Segmentation


## ___Objective___
Using Computer Vision Model to detect 3 classes (3XP, Pinot_Gregio, Villa_Maria) of White Wines for Inventory Management



## ___Timeframe___
	25th September 2025 - 09th November 2025



## Challenges:

1. Decision to go with Object Detection or Segmentation

	  + **Solution**: Since the bottles in the image are very closely packed so decided to go with Segmentation task for better detecting the object for Inventory Counting
	  + **Result**: Better Counting Results in Segmentation than in Object Detection Model

2. Limited Dataset
      + **Solution**: Data Augmentation - Same images is being augmented by adding blurness, brightness and noise to the background - resulting in 3X of the source dataset 

3. Manual Data Labelling
	+ **Solution**: CVAT with SAM (Segment Anything Model) helps a lot in Annotating almost ~1000 images
   
4. Choosing the Model
   
   + **Solution**: The main objective of the Project is to run the model on Edge Device like Smartphone or Rasberry PI, therefore decided to go with the least resource required model - YOLOv8n




## Approaches Taken:

> **Generalization**: First try to make model generalize on these 3 wines by providing these wines images in different background

> **Overfitting**: But then shifted approach to Overfit the model to a particular background for better Results
	
	
## Tools Used:
**Data ANnotation**
	labelImg
	labelme
	CVAT
	Roboflow
	
**Model Building**:
	Google Colab
	Ultralytics Hub

**3rd Party App**:
	Ultralytics Mobile App - 
	**NOTE**: For now the app is only limited to run Object Detection Model - No Provision to Run Segmentation Model
	When try to run a bit better Output Object Detection Model, the Model didnt able to detect objects in Real Time in the app.
	

**App Running and Code Deployment**:
	VS Code
	huggingface cli
	git
	
## Learning:
	1. Labelling Trick: If we augmented the data with any of the above mentioned ways, we can use the same labels as original image because the position of the object doesnt change (my thought :))
	
	2. Dynamic Learning Rate - cos_lr =True;  If we fix the LR to one value that doesnot changes then if we keep it to higher end then the Learning will fluctuate a lot point will fluctuate a lot while if we keep to a lower value then it will take a lot of time to learn… SO we want it to be variable, to start high (to catch the basic details as early as possible) and then slow down to more focus on finer details..
		Step Decay -- let the LR to fall after certain epoch -- which will also be a problem bevause it might give shock to the model of sudden drop in LR..
		COSINE -- Therefore Cosine Decay… 

	3. Evaluation Metric: train/box_loss -- TO locate the object in the space -- create a bounding box around the object -- Binary Cross Entropy at the end point of model prediction -- Loss calculated in the model prediction of box and ground truth box
							train/seg_loss -- boundaries of the object -- how does the object look like
							train/cls_loss -- which object -- identifying the class of the object -- whether 3xp or villa
							train/dfl_loss -- Distributed Focal Loss -- DFL focuses on selecting the most accurate boundary from this probability distribution.  
							Segmentation Loss VS DFL
								Segmentation Loss is more like checking whether this particular pixel belongs to the object or not -- Creating the mask of the object
								DFL -- Box precision — how tightly the bounding rectangle fits			
								Segmentation loss shapes the mask (pixel-by-pixel object outline), while DFL sharpens the box edges (coordinate precision) — both relate to object shape, but at different representation levels and for different purposes.

**Boring Part**:
	Manual Data Labelling

**Fun Part**:
	Testing the results
	Making Output work to some extent :)
	

## LIMITATION:
			Human Outperforms Model in cases:
				Limited Visibility:
					Like If the bottle is half filled or hidden behind another bottle or stacked very close to each other, model failed to correctly provide the results  
					
				Objects Shape and Design Change:
					IF tomorrow the shape of bottle changed or even if the product is totally replaced with a different brand, then retraining or again fine tunning will be required
					
					
## Further Enhancements:
		1. Model is too much Overfitted to provided images only (train and validation images), therefore even if we have provided a new image with the same background, the             model failed to detect / have mislabelled some of the objects as well
		**Problem**: Since this is the smallest model (YOLOv8n) therefore even with the most efficient results, the model failed to capture finer details.
		**Solution**: Try with a bit better model like YOLOv8s, or YOLOv8m
		
		2. OCR - Post Training Method -- Method to detect naming on the label and then delibrately tell model that the label says Villa so its Villa 
		
		3. Increasing the Batch Size -- 16 - 32/64  - Large batches → gradients are averaged over more examples → smoother, more reliable update direction.
		
		4. Increase Image Size -- 640 -- 800 -- Higher Resolution preserves bottle text and logo details
	
		5. Ensemble / Pseudo-Label Boost -- Manually label the wrong predicted image done by the model, correct them, label them and send it back to the model in its training. 
		
		6. CBAM -- Adding Attention Block to better identify the object
		
Some of the testing material:
		OneNote - AI Models - YOLO_model_ft
		YOLO_Segmentation_Testing_0311
