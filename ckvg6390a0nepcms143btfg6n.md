## Add anaconda to right-click menu in windows

## The problem
Anaconda prompt is my go-to solution for anything and everything python or data science-related. It is tailored for ease of use and reliability. In windows usually, we have to open the anaconda prompt and then navigate to the folder we want to do our project and then start working on our project. You see this is quite a tedious process and after some time you will get fed up.

## The Solution
We can easily overcome this problem by opening the anaconda prompt using the windows right-click menu.
Adding anaconda to the windows right-click menu is quite easy.

## STEP - 1 Run regedit.exe
Press `Windows key + r` to open the run window and then type in `regedit.exe`
and press ok.

![1 run the command.png](https://cdn.hashnode.com/res/hashnode/image/upload/v1635739945256/4IFAVqktg.png)

## STEP - 2 
Navigate to `HKEY_CLASSES_ROOT > Directory > Background > shell`

## STEP - 3
Add a key named AnacondaPrompt and set its value to "Anaconda Prompt Here" (or anything you'd like it to appear as in the right-click context menu)

![2 add new key.png](https://cdn.hashnode.com/res/hashnode/image/upload/v1635739997343/vRdj5RMcC.png)

![3 add key value.png](https://cdn.hashnode.com/res/hashnode/image/upload/v1635740096853/3aoG_AW5g.png)

## STEP - 4
Add a key under this key, called command, and set its value to 
```shell
cmd.exe /K C:\Anaconda3\Scripts\activate.bat
```
(may have to change the activate.bat file to wherever your Anaconda is installed)


![4 add command.png](https://cdn.hashnode.com/res/hashnode/image/upload/v1635740157816/jhbu89IMN.png)

## STEP - 5 

Now you can open your anaconda prompt by right-clicking your desired location.


![5 Anconda in right click menu.png](https://cdn.hashnode.com/res/hashnode/image/upload/v1635740820422/fEe959EVE.png)

