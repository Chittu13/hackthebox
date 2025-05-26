```
tar -xf firefox-<latest>.tar.xz
sudo mv firefox /opt/firefox-nightly
sudo ln -s /opt/firefox-nightly/firefox /usr/local/bin/firefox-nightly
sudo rm /usr/bin/firefox
sudo ln -s /opt/firefox-nightly/firefox /usr/bin/firefox
firefox --version
firefox
```
