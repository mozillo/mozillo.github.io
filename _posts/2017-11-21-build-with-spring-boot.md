---
layout: post
title:  "用SpringBoot构建JavaAPP-上传提取Emails"
date:   2017-11-08 22:21:49
categories: java
tags: Java 感悟
---

构建一个JavaWeb项目变得很简单。现在只需要安装Spring Boot。

如果加上上传与解析的话。可以用来做一些文本处理的事情，比如提取Email列表。

{% highlight java %}
package com.jingqi100.jqcloud.api;

import org.springframework.web.bind.annotation.GetMapping;
import org.springframework.web.bind.annotation.RequestMapping;
import org.springframework.web.bind.annotation.RequestParam;
import org.springframework.web.bind.annotation.RestController;

import java.io.*;
import java.util.ArrayList;
import java.util.List;
import java.util.regex.Matcher;
import java.util.regex.Pattern;

@RequestMapping(value = "/parse")
@RestController
public class ParseController {

    @GetMapping(value = "/text")
    public Object[] textParse(@RequestParam(value = "path") String path) {
        ArrayList<String> all = new ArrayList();
        String newNath = "/Users/mot/Desktop/word2txt/";
        File folder = new File(newNath);
        File[] listOfFiles = folder.listFiles();
        for(int i=0; i<listOfFiles.length; i++) {
            if (listOfFiles[i].isFile()) {
                String tmpFile = newNath + listOfFiles[i].getName();
                this.parseFile(tmpFile, all);
            }
        }

        return all.toArray();
    }

//    @GetMapping(value = "/eml")
//    public void emlParse() {
//        MimeMessage eml = new MimeMessage();
//    }

    public Object[] parseFile(String filePath, ArrayList all) {
        System.out.println("#########################Start scan: "+filePath);
        Integer counter = 0;
        List<String> list = new ArrayList<>();

        FileInputStream fis = null;
        try {
            fis = new FileInputStream(filePath);
        } catch (FileNotFoundException e) {
            // e.printStackTrace();
        }

        InputStreamReader isr = null;
        try {
            isr = new InputStreamReader(fis,"UTF-8");
        } catch (UnsupportedEncodingException e) {
            // e.printStackTrace();
        }
        BufferedReader br = new BufferedReader(isr);

        try {
            while(br.readLine() != null) {
                String str = br.readLine();
                String email = this.getEmailFromString(str);
                if (this.isEmail(email)) {
                    counter++;
                    all.add(email.trim());
                    System.out.format("Found email!!!!!: %s \n", email.trim());
                } else {
                    // System.out.format("Found line: %s \n", email);
                }
            }
            fis.close();
        } catch (IOException e) {
            // e.printStackTrace();
        }

        System.out.format("Get row count: %d \n", counter);

        return list.toArray();
    }


    public boolean isEmail(String email){
        if (null == email || "".equals(email)) return false;
        Pattern p = Pattern.compile("([a-zA-Z0-9\\.\\_\\%\\-\\_]+@[a-zA-Z0-9\\.\\-\\_]+\\.[a-zA-Z0-9\\_\\-]+)\\s{0,}", Pattern.MULTILINE);
        Matcher m = p.matcher(email);
        return m.matches();
    }

    public String getEmailFromString(String string) {
        if (string == "" || string == null) {
            return "";
        }
        Pattern p = Pattern.compile("([a-zA-Z0-9\\.\\_\\%\\-\\_]+@[a-zA-Z0-9\\.\\-\\_]+\\.[a-zA-Z0-9\\_\\-]+)\\s{0,}", Pattern.MULTILINE);
        Matcher m = p.matcher(string);

        if (m.find()) {
            return m.group(0);
        }
        return "";
    }
}
{% endhighlight %}
