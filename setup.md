---
layout: page
title: Setup
---

{% comment %} Setup {% endcomment %}
<h2 id="setup">Setup</h2>
<p>
  To participate in this workshop, you will need access to UF Hipergator and the software described below.
</p>
<p>
  We maintain a list of common issues that occur during installation as a reference for instructors that may be useful on the
  <a href = "{{ site.baseSite }}{{ site.faq }}">FAQ page</a>.
</p>


{% comment %} UF VPN Access {% endcomment %}
<div id="vpn" markdown="1">
### Connecting to Gatorlink VPN
Gatorlink VPN is required to access University if Florida Research Computing OnDemand
service, which will provide access to HiperGator cluster.
1. Visit [UF VPN webpage (vpn.ufl.edu)](https://vpn.ufl.edu){: target="_blank"}.
2. Login using the username and password that was emailed to you. [Note] For participants associated with University of Florida, please use your regular GatorLink account and password to login. 
3. In the next page, download **Cisco Anyconnect** software and install it.
4. Once installed, run **Cisco Anyconnect** and click **Connect**.
5. Enter the username and password provided and click **OK**. 
You should now be connected to Gatorlink VPN.
</div>

{% comment %} Hipergator Access {% endcomment %}
<div id="shell" markdown="1">
### Connecting to HiperGator
Make sure you are connected to Gatorlink VPN before the following steps.
1. In you web browser, navigate to [UFRC OnDemand (ood.rc.ufl.edu)](https://ood.rc.ufl.edu/){: target="_blank"}.
2. Login using provided username and password. You will be redirected to UFRC OnDemand homepage.
3. To connect to remote shell, click on **Clusters** in navbar and click 
**Hipergator Shell Access**. Note that the shell will open in new tab.
4. To view and transfer your files, click on **Files** in navbar 
in UFRC OnDemand homepage and click **/blue/general_workshop**.
Double click on directory named as your username to find your files.
</div>

{% comment %} Text editor {% endcomment %}
{% include setup/editor.html %}

{% comment %} Nanopore community {% endcomment %}
<div id="community" markdown="1">
### Join Nanopore Community 
Creating an account for the Nanopore community is required to access the protocols and software dowloads such as MinKNOW, the operating software that drives nanopore sequencing devices. 
Community also provide the online training, and plateform for discussion whithin the community.
1. In your web browser, navigate to [Log in page](https://id.nanoporetech.com/idp/SSO.saml2?SAMLRequest=jZLdS8MwFMX%2Flb7lqV%2FZ1rrQFsqGMJgfrOqDL5KldyzQJDU3Ufff23aIiii%2BXs7vnMO9t0Cuup7V3h31Dp49oAtqRLBOGr0yGr0C24B9kQLud9uSHJ3rkcWxOnEhjNcu0lyb3lhwII6RMCpGNDEXSIL14CY1H60%2BQdn%2BJGTbx01zE41lKAk265I8pft2P7%2FYJ2EyX9Jwnok85MssDZc0WxzocpZnfDZIET1sNDquXUloQmmY5CHN7xLKaM4WySMJHsDiVIFGCQneVKeRjUkl8VYzw1Ei01wBMidYU19t2SBk%2FGMLX5H%2Bb6a3xhlhOlIVo5pN7Wz1z50pcLzljhfxV7g4X%2Bh6CNusb00nxSmou868rixwByVx1gMJLo1V3P1eL43SaSLb8DBJmdfYg5AHCS2Jq3Po90%2Bo3gE%3D&SigAlg=http%3A%2F%2Fwww.w3.org%2F2000%2F09%2Fxmldsig%23rsa-sha1&Signature=Fiu%2BMIqwge3TF%2FdfZTpOuYbInmm0OJgijG3mJkaNebbn3exqdPPsfnmuXJvrhY3cIZMmnasyc1Kwmecc180Bv4TxQFxsiA%2B9tlCp5htjqzyuP6hWPW8HwrcFVZk3SNp%2FLvw9R4S03tuPBZNk8B2l3LiTXsIrib3WHiMoQFl8za%2F7sgU1tqpKJV7Op6XdyEQSQfBwe1usSRd5bpe3MQl49q83DIkpmTH7bnU5eXgaHgIRoSVy1XCSJXZ6IADIEd4K0eFnbvwYBtYHFf84UQoXyggAvN4zLoNghx5Os%2FbnNbMKYDTsLrYiMfqANIIqYdBy9nz6NitAqCiovqOU%2BdIHQw%3D%3D){: target="_blank"}. 
2. Click **Register** at the top right corner to register a new account. 
<img src="fig/ONT_login.png" align="center" width="500">
3. Fill in the information that required for creating an account.
<img src="fig/ONT_register.png" align="center" width="500">
4. System will ask you to provide more detail about you. 
<img src="fig/ONT_detail.png" align="center">
5. Click submit, and then a verification email will be sent to the email you provide at step 3.
<img src="fig/ONT_verify.png" align="center">
6. Find the verification email in your email (could be in the Junk Email), and then **Click To Verify**
<img src="fig/ONT_verify2.png" align="center">
7. Please set your password. 
<img src="fig/ONT_password.png" align="center">
8. You will be redirect to the ONT home page. Click **Community**. If you are not at the ONT home page after password setting, navigate yourself to 
[Community](https://community.nanoporetech.com/). After login, please proceed to **Download MinKnow**. 
<img src="fig/ONT_home.png" align="center" width="500">
</div>

{% comment %} Download MinKNOW {% endcomment %}
<div id="software" markdown="1">
### Download MinKNOW
Now you can access the Nanopore community, so we need to download MinKNOW, the operating software that drives nanopore sequencing devices.
Also, feel free to explore the community. 
1. Click on **Software Downloads** on the right pannel.
<img src="fig/Community_download.png" align="center" width="700">
2. You will be direct to *Software Downloads* page. Scroll down to find **MinION Software**(MinKNOW). Please download the software according to your laptop operation system, and
and you can follow the on-screen instructions by clicking **Installation guide** next to the **Download**.

[Note] Please **DO NOT** download MinION Mk1C software. It is a different seqeunce device with touchscreen, which is not used in this workshop. 
<img src="fig/Community_MinION.png" align="center" width="700">
3. Click on **MinKNOW icon** on your desktop
4. You will need to log in before start sequecning
<img src="fig/MinION_login.png" align="center" width="700">
5. Once you get in the interface, the system will walk you through the menu.
<img src="fig/MinION_tutorial.png" align="center" width="700">
</div>

> ## MinKNOW protocol
> You can find complete protocol of sequencing software [here](https://community.nanoporetech.com/docs/prepare/library_prep_protocols/experiment-companion-minknow/v/mke_1013_v1_revch_11apr2016)
{: .tips}
