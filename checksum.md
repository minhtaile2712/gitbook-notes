# Checksum

{% tabs %}
{% tab title="Linux" %}
Run this command in your terminal in the directory the iso was downloaded to verify the SHA256 checksum:

```
echo "c396e956a9f52c418397867d1ea5c0cf1a99a49dcf648b086d2fb762330cc88d *ubuntu-22.04.1-desktop-amd64.iso" | shasum -a 256 --check
```

You should get the following output:

```
ubuntu-22.04.1-desktop-amd64.iso: OK
```

Learn more [here](https://ubuntu.com/tutorials/how-to-verify-ubuntu)
{% endtab %}

{% tab title="Windows" %}
To be defined
{% endtab %}
{% endtabs %}
