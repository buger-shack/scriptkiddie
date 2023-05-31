# Forgot password of file ?

> In case you ever forget the password for any Microsoft Office file (pptx/xlsx/docx) you locked there is a [tool](https://github.com/nedlir/OfficerBreaker) that removes the password from the file without cracking it.

## OfficeBreaker

{% embed url="https://github.com/nedlir/OfficerBreaker" %}

All pptx/xlsx/docx files are part of the Office Open XML format family (for further reading please refer to [OOXML Format Family -- ISO/IEC 29500 and ECMA 376 ](https://www.loc.gov/preservation/digital/formats/fdd/fdd000395.shtml)).

For example, a standard _**.pptx**_ file will have the following file tree structure:

```
myFile.pptx
.
├── docProps
│   ├── app.xml
│   ├── core.xml
│   ├── custom.xml
│   └── thumbnail.jpeg
├── ppt
│   ├── handoutMasters
│   ├── media
│   ├── media
│   ...
│   ...
│   ...
│   └── presentation.xml
├── _rels
│   ├── .rels
└── [Content_Types].xml
```

We can see this structure by opening the file using a program like [7zip](https://www.7-zip.org/) or by changing the filetype to _**.zip**_ and then opening it.

Each OOXML file type contains an _**.xml**_ file with settings and preferences, including read-only protection. In our example the security element will be located inside **presentation.xml** file which is located inside the `ppt` folder of **myFile.pptx**.

Inside **presentation.xml** there is a specific element we will focus on called **p:modifyVerifier** which should look like this:

```python
<p:modifyVerifier cryptProviderType="rsaAES" cryptAlgorithmClass="hash" cryptAlgorithmType="typeAny" cryptAlgorithmSid="14" spinCount="100000" saltData="3R1lmtJocEj5GzEGRn3MHA==" hashData="iR0jIUtVcGsTx62z/hqcbzaReLJemv$eZyqTlpWhl0Lph+osBKEiEYmyReJHmypMy6wj+VFmDGuNZvsMA9tX9g=="/>
```

The file editing is protected by a password which was salted and hashed which makes it nearly impossible to crack within reasonable time. But instead of trying to crack the password, we can just... Remove it. :shrug:

Turns out that simply deleting the security element **p:modifyVerifier** as a whole will make **myFile.pptx** behave as if it never had any password at all. This kind of security measure is a bit like the photo in the title of this repository - a good lock placed on the door handle... :sweat\_smile:

The program will create a copy of **presentation.xml**, parse it and delete the security element. Once the element is deleted, the _copied_ **presentation.xml** will be replaced with the _original_ **presentation.xml** which will effectively remove the password from **myFile.pptx.**

What makes this whole thing worse is the fact that we could simply remove the password created by the author, alter the file in some way and then return the original password of the author by inserting the same security element which was removed. This hurts the integrity of the whole OOXML format family.

### Future changes / possible deprecation

{% hint style="warning" %}
Future versions of OOXML file type may make drastic changes of naming convention of elements or/and structure of the folders or/and files.&#x20;

**This might make this project deprecated** but not obsolete if the same security measures will be taken in future versions. If a "lock with different name" (the hashing) will be placed on the "door handle" (removable `.xml` element), the file still could be altered/edited without a password. All that's needed is to find the new security element and simply remove it from the file.
{% endhint %}
