# Firebase notes

## Overview

Firebase can be used with vanilla JS as the front-end. But it is often used professionaly with front-end framework like React, Angular, Flutter, Vue

Firebase is an extension of Google Cloud platform. It make it easy for your font-end app to connect to back-end cloud infrastructures on Google Cloud.

## Bugs

Khi add firebase plugin nếu gặp lỗi `Because firebase_auth depends on firebase_auth, version solving failed.` => Trong `pubspec.yaml` có name cái name này không được giống name của firebase plugin.

## FAQs

[Firebase vs GCP](https://www.geeksforgeeks.org/firebase-vs-gcp/)

Warning: Pub installs executables into $HOME/.pub-cache/bin, which is not on your path.
You can fix that by adding this to your shell's config file (.zshrc, .bashrc, .bash_profile, etc.):

  `export PATH="$PATH":"$HOME/.pub-cache/bin"`

  Muốn coi version của Xcode thì phải vào Application coi. Không coi bằng terminal.
  
## Authentication

## Terminology

BaaS: Backend-as-a-Service meaning UI developer không cần bận tâm viết code server-side backend mà trả tiền cho bên service làm dùm.

GCP: Google Cloud Platform

Firebase project: là cái mình tạo trên console online trên browser. Firebase project id must be unique GLOBALLY not just in your google account.

