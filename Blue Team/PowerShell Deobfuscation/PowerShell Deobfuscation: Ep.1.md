This is the first laboratory and it is the easiest one however I will give you some amazing tips I would have liked to know. Something to consider is that all the labs were created using "Powershell Invoke Obfuscation", which mean, all the labs are different and if you end or reset it, a new code will be created, a new encode will be added or the initial one will be totally different but the important thing is the methodology, if you understand how the script works, you can decode any script. Alright, with that been said, let's start!

You are going to start with a dropper.ps1 file and initially it will be unreadable but every time an script or code is "obfuscated" it must has an execution instruction, for example, Invoke-Expression, iex, etc., we have different ways to deobfuscate, manually with CyberChef or a custom script or executing the dropper.ps1 removing the execution instructions.

![Image Description](Uploads/Ep.1%20dropper.png)
