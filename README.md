# WD-14-TF-Tagger

This repository is a batch tagger adapted from the hugginface space that works using the Tensorflow models made by SmilingWolf support for **Convnextv2**, **Convnext_tagger_v2**, **Swinv2** and **ViTv2**. It will generate a txt file with the same name of the image with the prediction results inside.

It can be run on both GPU and CPU.

## Installation
To start using clone the repository:

    git clone https://github.com/LtSRvan/WD-14-TF-Tagger

CD into the folder:

    cd WD-14-TF-Tagger
    
Run the **create-venv.bat** file. it will take a few minutes to finish:

    create-venv.bat

Once it's done install the **requirements.txt**:

    pip install -r requirements.txt

You have to download the variables folder and the saved_model.pb, put both in a folder with the name of the model that you downloaded. Yo can find the [Models](https://huggingface.co/SmilingWolf) here.
For the [selected_tags.csv](https://huggingface.co/SmilingWolf/wd-v1-4-swinv2-tagger-v2/resolve/main/selected_tags.csv) you have to put that in the Models folder or let the script download it for you

## Usage

Once you activate your venv for a basic usage type (by default uses the GPU):

    python tagger.py --input "your/folder/path" --model /path/to/your/model/folder --general_score 0.5
   
With this command it will tag your images and write the result in a TXT file with the same name as the file

## Comand line args

### --input
Specify the directory where your images to be processed are located. Default ./Test

### --output 
Specify the directory where the results are going to be stored. Default will be same as input unless you specify a directory. If it not exist the script will create it

### --label
By default, it assumes that 'selected_tags.csv' is in the Models folder. If it has a different name or is on another directory use this argument to specify.

### --model
As mentioned at the beginning you can use 'Convnextv2', 'Convnext_tagger_v2', 'Swinv2' or 'ViTv2'. It's important to actually download the variables folder and the saved_model.pb, otherwise it will not work. I suggest you to put those files in folders with the name of the model so it easy to recognize and to keep everything in place. Example Path "C:\Documents\WD-14-TF-Tagger\Models\SwinV2"

### --general_score
Specify the minimum 'confidence' percentage for a tag in the prediction. The lower the number, the more tags will appear, but they may be redundant or not entirely accurate to what is seen in the image.
By default it's set on 0.5
 
### --character_score
Similar to the previous one, specify the minimum 'confidence' level for a character in the prediction. The lower the number, the more likely it is that characters unrelated to the image in question may appear.
By default it's set on 0.85

### --batch_size 
The higher the faster but also it will use more VRAM. By default it's set to 32 (with that it uses 8.8GB of VRAM)

### --add_keyword
~~If you want to use the tag format used in [MF-Bofuri](https://huggingface.co/MyneFactory/MF-Bofuri) use this. It will append the keyword at the beggining of the text. Keep in mind that to use it this way, you must have your dataset well organized by characters/artists/style so that it does not 'contaminate' other images that do not require that specific addition.~~ To be re-implemented alongside with the auto-download option

## Examples of **general_score**, **character_score** and **add_keyword**

### General_score

![Kazuma_general_score](/Examples/Kazuma_general_score.jpg)

As I mentioned earlier the lower the number the more irrelevant or redundant tags will appear, you have to do some experiments to find what is more suitable for you

### Character_score

![Power_character_score](/Examples/Chainsaw_character_score.jpg)

Normally you won't need this option, just in some cases that maybe some characters are not detected you can lower a bit the number but be careful, as you can see it will start adding characters that are not there or there's not enough information of them in the picture so adding them will be useless and even bad for the model that you're training

~~### Add_keyword~~

~~![Kazuma_add_keyword](/Examples/Kazuma_keyword.jpg)~~

~~This is just to showcase how it will look when you use this option, in the example that I refenced before ([MF-Bofuri](https://huggingface.co/MyneFactory/MF-Bofuri)) things like 'BoMaple' or 'BoSally' are used to make reference of the characters. You can add whatever tag/s you want. If you want to use multiple tags use quotation marks to add them, like this --add_keyword **"Konosuba, anime"**~~
