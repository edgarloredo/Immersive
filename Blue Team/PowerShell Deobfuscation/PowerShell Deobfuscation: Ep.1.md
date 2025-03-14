This is the first laboratory and it is the easiest one however I will give you some amazing tips I would have liked to know. Something to consider is that all the labs were created using "Powershell Invoke Obfuscation", which mean, all the labs are different and if you end or reset it, a new code will be created, a new encode will be added or the initial one will be totally different but the important thing is the methodology, if you understand how the script works, you can decode any script. Alright, with that been said, let's start!

You are going to start with a dropper.ps1 file and initially it will be unreadable but every time an script or code is "obfuscated" it must has an execution instruction, for example, Invoke-Expression, iex, etc., we have different ways to deobfuscate, manually with CyberChef or a custom script or executing the dropper.ps1 removing the execution instructions.

![Image Description](/Blue%20Team/Uploads/Ep.1%20dropper.png)

Here you have two options, you can execute the PowerShell script and remove the execution part or you can upload the file in CyberChef and perform different Operations. I am going to show you both scenarios.

1. CyberChef
When you upload the file or copy/paste it, you will see the content and you need to analyze the script to determine the best Operations, in this case we can see **FrOmBASE64sTRiNG** which indicates the string was encoded in base64, we are going to add *From Base64* in the Recipe but we only see random characters, if we continue reading the code, the content was compressed using **dEFLAtEStreAm** so, we need to perform the opposite and we will use *Raw Inflate* but for some reason it did not work, why? Because we only need to add the base64 string so we need to remove everything and then we will see the original raw data.

![Image Description](/Blue%20Team/Uploads/original.png)
![Image Description](/Blue%20Team/Uploads/obase64.png)
![Image Description](/Blue%20Team/Uploads/oinflate.png)
![Image Description](/Blue%20Team/Uploads/final.png)
