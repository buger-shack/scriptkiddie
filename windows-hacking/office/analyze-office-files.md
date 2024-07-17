# Analyze office files



<figure><img src="../../.gitbook/assets/the-simpsons-homer-simpson.gif" alt="" width="249"><figcaption></figcaption></figure>

## Basics

{% hint style="info" %}
There are **two generations** of **Office** **file** **format**:&#x20;

* the **OLE** formats (file extensions like RTF, DOC, XLS, PPT),&#x20;
* the "**Office Open XML**" formats (file extensions that include DOCX, XLSX, PPTX).&#x20;

Both formats are **structured**, compound file binary formats that enable Linked or Embedded content (Objects).&#x20;

OOXML files are actually **zip** file containers, meaning that one of the easiest ways to check for hidden data is to simply **`unzip`** the document

```
unzip file.docx
```
{% endhint %}

## Are they really malicious ?

```bash
# install basic tools
sudo pip3 install -U oletools

# oleid : analyze OLE files to detect specific characteristics usually found in malicious files
oleid file.xls

# upload the file to virustotal and see...
# https://www.virustotal.com/gui/home/upload
```

### Macros

<pre class="language-bash"><code class="lang-bash">#
# oledump
#
wget https://raw.githubusercontent.com/DidierStevens/DidierStevensSuite/master/oledump.py
python3 oledump.py -h

# List all OLE2 streams present in file.xls
python3 oledump.py file.xls -i

# Extract VBA source code from stream 3 in file.xls
<strong>python3 oledump.py file.xls -s 3 -v
</strong><strong>
</strong><strong># Find obfuscated URLs in file.xls macros
</strong>python3 oledump.py file.xls -p plugin_http_heuristics

# 
# olevba
#
# Extract VBA macros in clear text with deobfuscation and analysis
olevba file.doc
</code></pre>
