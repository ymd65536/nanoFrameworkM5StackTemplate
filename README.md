## Overview

このリポジトリはnanoFrameworkのプロジェクトをVisual Studio Code(VSCode)で使うためのリポジトリテンプレートです。`Use This Template`をクリックして、VSCodeでプロジェクトを作成してください。

## Target

このプロジェクトはM5Stack Core2をターゲットにしています。
M5Stack Core2はESP32を搭載したマイクロコントローラーで、nanoFrameworkを使用してプログラムできます。また、M5Stack Fire v2.7も同様にESP32を搭載しているため、同じプロジェクトで動作します。

具体的には以下の環境で動作します。

- OS
  - Windows 11 Pro 25H2
  - OS build 26200.8246

※macOS(Macbook Apple Silicon)ではビルドは可能ですが、ビルドしたファイルはM5Stack Core2やM5Stack Fire v2.7では動作しません。

- hardware
  - [M5Stack Core2](https://docs.m5stack.com/ja/core/core2)
  - [M5Stack Fire v2.7](https://docs.m5stack.com/ja/core/fire)

- deploy
  - nanoff 2.5.143+9f97ac82d9  

- runtime
  - dotnet --version
  - 10.0.107

- VSCode Extension
  - nanoframework.vscode-nanoframework

## Setup bootloader

この手順ではnanoFrameworkでビルドしたプログラムを動かすためのブートローダをM5Stackデバイスに書き込む方法を説明します。

M5Stackデバイスに対応したブートローダを書き込むには、以下のコマンドを使用します。
書き込みの際は、デバイスが接続されているシリアルポートを指定する必要があります。

デバイス情報を確認するセクションで確認したターゲット名を`--target`オプションに指定してください。

ターゲット名の一覧は`ターゲットを認識する`セクションを参照してください。

`[serialport]`には`nanoff --listports`で確認したシリアルポートを指定してください。

```bash
nanoff --update --target M5Core2 --fwversion 1.16.0.568 --serialport [serialport] --baud 115200 --masserase
```

--fwversionはわからない場合は`--listtargets`で確認してください。具体的には以下のコマンドを実行して、ターゲット名と対応するブートローダのバージョンを確認してください。

```bash
nanoff --listtargets
```

実行結果

```text
  M5Core2
    1.16.0.568
    1.16.0.567
    1.16.0.563
```

## Setup

VSCodeでnanoFrameworkのプロジェクトを作成するには、まずは`nanoff`というコマンドラインツールをインストールする必要があります。以下のコマンドを実行して、`nanoff`をインストールしてください。（インストールのため最初の一度だけ実行）

```bash
dotnet tool install -g nanoff
```

次に、VSCodeでプロジェクトを作成するための拡張機能をインストールします。VSCodeの拡張機能マーケットプレイスで`nanoframework.vscode-nanoframework`を検索して、インストールしてください。

最後にプロジェクトを設定するために`nfproj`と`sln`を修正します。

まずは`nfproj`の`RootNamespace`と`AssemblyName`を変更します。

```xml
<RootNamespace>nanoFrameworkM5StackTemplate</RootNamespace>
<AssemblyName>nanoFrameworkM5StackTemplate</AssemblyName>
```

`sln`では`nfproj`ファイルが読み込まれます。`nfproj`ファイルの名前を変更してください。
以下の例では`nanoFrameworkM5StackTemplate.nfproj`を`MyProject.nfproj`に変更しています。

変更前

```sln
Project("{11A8DD76-328B-46DF-9F39-F559912D0360}") = "nanoFrameworkM5StackTemplate", "nanoFrameworkM5StackTemplate.nfproj", "{AE52F6E3-F33F-4818-A982-44922EA0060F}"
```

変更後

```sln
Project("{11A8DD76-328B-46DF-9F39-F559912D0360}") = "MyProject", "MyProject.nfproj", "{AE52F6E3-F33F-4818-A982-44922EA0060F}"
```

これでプロジェクトのセットアップは完了です。VSCodeでプロジェクトを開いて、ビルドやデプロイを行うことができます。

## Usage

VSCodeでは主にnanoffを使用してビルドやデプロイを行います。

おおまかな流れは以下の通りです。

1. シリアルポート番号（COM番号）の確認
2. 拡張機能によるビルド

## show serial port

デプロイ前に、接続されているM5Stackデバイスのシリアルポート番号を確認してください。

確認する方法は2つあります。

コマンドのみを使用して確認する。

```bash
nanoff --listports
```

デバイス名を含めて確認する。

```bash
nanoff --listdevices
```

実行結果

```text
.NET nanoFramework Firmware Flasher v2.5.143+9f97ac82d9
Copyright (C) 2019 .NET Foundation and nanoFramework project contributors


-- Connected .NET nanoFramework devices --
M5Core2 @ COM6
```
