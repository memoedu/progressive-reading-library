## 前言

**TCIA (The Cancer Imaging Archive)** 是一个包含常见肿瘤（肺癌、前列腺癌等）医学图像及相应临床信息（治疗方案细节、基因、病理等）的大规模公用数据库。其中的数据链接往往通过 `.tcia` 格式进行描述，你无法直接通过 http 协议下载它们，需要使用 NBIA 数据查询器来下载它们。

:::info

比如这个链接 https://www.cancerimagingarchive.net/wp-content/uploads/TCIA_LIDC-IDRI_20200921.tcia 这就是一个 tcia 下载链接。

:::

考虑到大部分数据科学家都喜欢使用 linux 服务器进行数据的日常管理，所以这篇文章我简单讲一讲如何在 Linux 服务器上使用 NBIA 来下载 TCIA 协议的数据。

---

## 下载 & 安装

首先下载并安装 NBIA，官方的文档如下： [NBIA Data Retriever Command-Line Interface Guide](https://wiki.cancerimagingarchive.net/display/NBIA/NBIA+Data+Retriever+Command-Line+Interface+Guide)

下载 & 安装的命令为

```bash
cd ~/download
wget -c https://cbiit-download.nci.nih.gov/nbia/releases/ForTCIA/NBIADataRetriever_4.4/nbia-data-retriever-4.4.2.deb
sudo dpkg -i nbia-data-retriever-4.4.2.deb
```

:::info

Ubuntu 下可执行文件会被安装到 `/opt/nbia-data-retriever` 下。CentOS 则是 `/opt/NBIADataRetriever/`

:::

---

## 使用方法

使用方法如下

```bash
/opt/nbia-data-retriever/nbia-data-retriever --cli <tcia 文件的绝对路径> -d <下载目录的绝对路径> -l <credential file的绝对路径> -v –f
```

输入后，同意基本协议后，NBIA 就会开始自动下载对应的文件。

其中 `credential file` 可有可无（根据数据本身的公开性而言），文件内容格式如下

```
userName=YourUserName
passWord=YourPassword
```

---

## 命令行参数

| Option                        | Description                                                  |
| :---------------------------- | :----------------------------------------------------------- |
| -c, -C, --cli, --CLI          | Indicates running as a CLI app                               |
| -cd, -CD, --CD or –cd         | Run with classic directory naming, which organizes files in a child folder under the destination folder as follows: Collection Name > Patient ID > Study Instance UID > Series Instance UID |
| -d, -D `<download directory>` | Required. The user must have write permission for the directory specified with this option. |
| -dd, -DD, --DD or –dd         | Run with descriptive directory naming, which is the default. A descriptive directory name organizes the files in a child folder under the destination folder as follows: Collection Name > Patient ID > Study Date + Study ID + Study Description (54 char max) + last 5 digits of Study Instance UID > Series Number + Series Description (54 char max) + last 5 digits of Series Instance UID |
| -f, -F                        | Skip the series the user does not have access to. Force the user to download the series if the user has access. Default is false. |
| -l `<credential file>`        | Required when the manifest file has series from restricted collections.Optional when the manifest file has series from public collections only. |
| -m, -M, --md5, –MD5           | Enable validation of the checksums of the downloaded files. Default is that checksums will not be validated. |
| -p, -P `<password>`           | Optional                                                     |
| -q, -Q                        | Quiet. Default is false.                                     |
| -u, -U `<user name>`          | Optional                                                     |
| -v, -V                        | Verbose. Default is false.                                   |





