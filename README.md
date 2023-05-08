# WD-14-TF-Tagger

This repository is a batch tagger adapted from the hugginface space that works using the Tensorflow models made by SmilingWolf. Support for **ConvNext**, **ConvNextV2**, **SwinV2** and **ViTv2**. It will generate a txt file with the same name of the image with the prediction results inside.

It can be run on both GPU and CPU. 
Now a Google Colab Notebook is avaliable for easy use <a href="https://colab.research.google.com/github/LtSRvan/WD-14-TF-Tagger/blob/main/WD_14_TF_Tagger.ipynb"><img src="https://colab.research.google.com/assets/colab-badge.svg" alt="Google Colab Logo"></a>

## Installation
To start using clone the repository:

    git clone https://github.com/LtSRvan/WD-14-TF-Tagger

CD into the folder:

    cd WD-14-TF-Tagger
    
Run the **create-venv.bat** file. it will take a few minutes to finish:

    create-venv.bat

Once it's done install the **requirements.txt**:

    pip install -r requirements.txt

You can find the [Models](https://huggingface.co/SmilingWolf) here. Download the saved_model.pb and the variables folder an put those on the same folder . If you don't want to download manually the script can do it for you, more info below
For the [selected_tags.csv](https://huggingface.co/SmilingWolf/wd-v1-4-swinv2-tagger-v2/resolve/main/selected_tags.csv) you have to put that in the Models folder or let the script download it for you

## Usage

Once you activate your venv for a basic usage type (by default uses the GPU):

    python tagger.py --input "your/folder/path" --model model_name --general_score 0.5
   
With this command it will tag your images and write the result in a TXT file with the same name as the file

## Comand line args

### --input
Specify the directory where your images to be processed are located. Default ./Test

### --output 
Specify the directory where the results are going to be stored. Default will be same as input unless you specify a directory. If it not exist the script will create it

### --label
By default, it assumes that 'selected_tags.csv' is in the Models folder. If it has a different name or is on another directory use this argument to specify.

### --model
As mentioned at the beginning you can use 'ConvNext', 'ConvNextV2', 'SwinV2' or 'ViTv2' (Only use the names). You have to put the path to the folder where the saved_model.pb and the variables folder are. In case you don't have any model downloaded you can usa the '--download' argument but remember to remove it if you are going to run with the same model again. Example Path "C:\Documents\WD-14-TF-Tagger\Models\SwinV2"

### --download
Use this argument to automatically download the corresponding model, only put the exact name of the model and the script will take care of the rest

### --general_score
Specify the minimum 'confidence' percentage for a tag in the prediction. The lower the number, the more tags will appear, but they may be redundant or not entirely accurate to what is seen in the image.
By default it's set on 0.5
 
### --character_score
Similar to the previous one, specify the minimum 'confidence' level for a character in the prediction. The lower the number, the more likely it is that characters unrelated to the image in question may appear.
By default it's set on 0.85

### --batch_size 
The higher the faster but also it will use more VRAM. By default it's set to 32 (with that it uses 8.8GB of VRAM)

### --add_initial_keyword and --add_final_keyword
If you want to use the tag format used in [MF-Bofuri](https://huggingface.co/MyneFactory/MF-Bofuri) use this. It will append the keyword at the beggining or the end of the text. Keep in mind that to use it this way, you must have your dataset well organized by characters/artists/style so that it does not 'contaminate' other images that do not require that specific addition. If the prediction detects a specific character is going to also be on the results, it will be interesting to see how that affects training (BoSally + Sally). If you don't want that remove the {character_tags}, from lines **181**, **183** and **185**

## Examples of **general_score**, **character_score** and **add_keyword**

### General_score

![Kazuma_general_score](/Examples/Kazuma_general_score.jpg)

As I mentioned earlier the lower the number the more irrelevant or redundant tags will appear, you have to do some experiments to find what is more suitable for you

### Character_score

![Power_character_score](/Examples/Chainsaw_character_score.jpg)

Normally you won't need this option, just in some cases that maybe some characters are not detected you can lower a bit the number but be careful, as you can see it will start adding characters that are not there or there's not enough information of them in the picture so adding them will be useless and even bad for the model that you're training

### Add_initial_keyword and add_final_keyword

~![Kazuma_add_keyword](/Examples/Kazuma_keyword.jpg)

This is just to showcase how it will look when you use this option, in the example that I refenced before ([MF-Bofuri](https://huggingface.co/MyneFactory/MF-Bofuri)) things like 'BoMaple' or 'BoSally' are used to make reference of the characters. You can add whatever tag/s you want. If you want to use multiple tags use quotation marks to add them, like this --add_initial_keyword **"Konosuba, anime"**
