This is the first laboratory and it is the easiest one however I will give you some amazing tips I would have liked to know. Something to consider is that all the labs were created using "Powershell Invoke Obfuscation", which mean, all the labs are different and if you end or reset it, a new code will be created, a new encode will be added or the initial one will be totally different but the important thing is the methodology, if you understand how the script works, you can decode any script. Alright, with that been said, let's start!

You are going to start with a dropper.ps1 file and initially it will be unreadable but every time an script or code is "obfuscated" it must has an execution instruction, for example, Invoke-Expression, iex, etc., we have different ways to deobfuscate, manually with CyberChef or a custom script or executing the dropper.ps1 removing the execution instructions.

![Image Description](/Blue%20Team/Uploads/Ep.1%20dropper.png)

Here you have two options, you can execute the PowerShell script and remove the execution part or you can upload the file in CyberChef and perform different Operations. I am going to show you both scenarios. As mentioned in the beginning, you need to find the execution instruction, could be very obvious or it could be obfuscate. In this case you can see at the beginning of the code &((VARIable '*mDR*').NAMe[3,11,2]-join'') and this is the execution, how will you know? Because you can run it in powershell.

The first part of the command it will give you the name of any variable with the word **mdr**, then it will only keep the value of *Name*, after that you need to select the characters, in this case 3 11 2, which are i e x (the positions of the string always start in 0), finally you need to join the value to create the string **iex**. So, now that you understand one of the multiple ways to obfuscate the execution instruction, we need to remove it from the Ps1 because we do not want to execute, we want to decode it.

We can execute dropper.ps1 and it will only do the deobfuscation and not the execution of the possible malicious content, and **voila!**, we have the original raw data.

![Image Description](/Blue%20Team/Uploads/manual.png)

![Image Description](/Blue%20Team/Uploads/iexmanual.png)

![Image Description](/Blue%20Team/Uploads/ps1manual.png)

1. Manual PS1 Execution
In my opinion the easiest way to deobfuscate as you just need to do minimum changes however you need to understand which parts of the code are required and which ones you can remove without affecting the logic.

3. CyberChef
When you upload the file or copy/paste it, you will see the content and you need to analyze the script to determine the best Operations, in this case we can see **FrOmBASE64sTRiNG** which indicates the string was encoded in base64, we are going to add *From Base64* in the Recipe but we only see random characters, if we continue reading the code, the content was compressed using **dEFLAtEStreAm** so, we need to perform the opposite and we will use *Raw Inflate* but for some reason it did not work, why? Because we only need to add the base64 string so we need to remove everything and then we will see the original raw data.

![Image Description](/Blue%20Team/Uploads/original.png)

![Image Description](/Blue%20Team/Uploads/obase64.png)

![Image Description](/Blue%20Team/Uploads/oinflate.png)

![Image Description](/Blue%20Team/Uploads/final.png)
