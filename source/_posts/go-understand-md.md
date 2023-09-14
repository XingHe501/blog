---
title: go_understand.md
description: ''
tag: go
date: 2023-09-14 18:43:36
tags:
categories:
---

```go
func main() {
	url := "http://192.168.1.17:8080/download/sango-resources_202309141501_DEV_NEW.zip"
	md5hash, err := downloadAndGetMD5(url)
	if err != nil {
		fmt.Println(err)
		return
	}
	fmt.Println(md5hash)

	md5hash1, err := downloadFile(url)
	if err != nil {
		fmt.Println(err)
		return
	}
	fmt.Println(md5hash1)
}
```


```go
func downloadFile(url string) (md5 string, err error) {
	filename := url[strings.LastIndex(url, "/")+1:]
	resp, err := http.Get(url)
	if err != nil {
		return
	}
	defer resp.Body.Close()

	file, err := os.CreateTemp(os.TempDir(), filename)
	if err != nil {
		return
	}
	defer os.Remove(file.Name())
	_, err = io.Copy(file, resp.Body)
	if err != nil {
		return
	}

	md5, err = getFileMD5(file.Name())
	return
}

func getFileMD5(filename string) (string, error) {
	file, err := os.Open(filename)
	if err != nil {
		return "", err
	}
	defer file.Close()

	md5hash := md5.New()
	_, err = io.Copy(md5hash, file)
	if err != nil {
		return "", err
	}
	currentPos, err := file.Seek(0, os.SEEK_CUR)
	fmt.Println("2CurrentPos:", currentPos)

	hash := md5hash.Sum(nil)
	hashStr := fmt.Sprintf("%x", hash)
	return hashStr, nil
}


```

```go

func downloadAndGetMD5(url string) (string, error) {
	filename := url[strings.LastIndex(url, "/")+1:]
	resp, err := http.Get(url)
	if err != nil {
		return "", err
	}
	defer resp.Body.Close()

	file, err := os.CreateTemp(os.TempDir(), filename)
	if err != nil {
		return "", err
	}
	defer os.Remove(file.Name())
	_, err = io.Copy(file, resp.Body)
	if err != nil {
		return "", err
	}

	currentPos, err := file.Seek(0, os.SEEK_CUR)
	fmt.Println("1CurrentPos:", currentPos)

	md5hash := md5.New()
	_, err = io.Copy(md5hash, file)
	if err != nil {
		return "", err
	}

	hash := md5hash.Sum(nil)
	hashStr := fmt.Sprintf("%x", hash)
	return hashStr, nil
}
```