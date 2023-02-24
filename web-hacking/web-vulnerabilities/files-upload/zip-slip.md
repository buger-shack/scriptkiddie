# 📦 ZIP Slip

{% hint style="info" %}
When a ZIP/archive file is automatically decompressed after the upload.

The vulnerability has been found in multiple ecosystems, including JavaScript, Ruby, .NET and Go, but is especially prevalent in Java, where there is no central library offering high level processing of archive (e.g. zip) files. The lack of such a library led to vulnerable code snippets being hand crafted and shared among developer communities such as [StackOverflow.](https://stackoverflow.com/questions/981578/how-to-unzip-files-recursively-in-java)

The vulnerability is exploited using a specially crafted archive that holds directory traversal filenames (e.g. ../../evil.sh). The Zip Slip vulnerability can affect numerous archive formats, including tar, jar, war, cpio, apk, rar and 7z.
{% endhint %}

{% embed url="https://github.com/snyk/zip-slip-vulnerability" %}

### Exploitation

{% embed url="https://github.com/ptoomey3/evilarc" %}
evilarc
{% endembed %}

```bash
python2 evilarc.py -p /etc/passwd $zip_input_file
```
