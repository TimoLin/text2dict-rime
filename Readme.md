text2dict-rime
==============
一个Python脚本，可以根据已有的中文文档来生成`Rime`自定义词库。

- 中文文档转为纯文本
- `jieba`分词提取词库信息
- `pypinyin`将分词结果转换为`pinyin`
- 生成`Rime`的自定义词库

## Python环境
基于`jieba`与`pypinyin`开发，安装依赖：
```sh
pip install -r requirements.txt
```

## 工作流示例
例如有一段文字是用Word写的，需要根据它创建自定义词库
1. 将`Word`文档导出为`txt`文件：
   - `导出`-`更改文件类型`-`纯文本`-`其他编码`-`UTF-8`-`确定`
   - 例如文件名为`foobar.txt`，放在`Run`目录下
2. 设置`jieba`分词的用户自定义词典，保证更准确的分词效果：
   - 修改示例目录`Run`中的`user.dict`
3. 运行`text2dict`：
   - 进入`Run`目录
   - 假设输出的词库文件名为`user-defined.dict.yaml`，执行：
     ```sh
     python3 ../src/text2dict.py -i foobar.txt -o user-defined.dict.yaml
     ```
4. 将自定义词库添加到`Rime`中
   - 进入用户文件夹，以`Linux`下`fcitx5-rime`为例
     ```sh
     cd  ~/.local/share/fcitx5/rime
     ```
   - 将`Step 3`中生成的`user-defined.dict.yaml`复制到该文件夹下
   - 假设使用的是雾凇拼音方案，编辑`rime_ice.dict.yaml`，在`import_tables`列表的最后添加自定义词库名（去掉文件名中的`.dict.yaml`），例如：
     ```yaml
     import_tables:
       - cn_dicts/8105     # 字表
       # - cn_dicts/41448  # 大字表（按需启用）
       - cn_dicts/base     # 基础词库
       - cn_dicts/ext      # 扩展词库
       - cn_dicts/tencent  # 腾讯词向量（大词库，部署时间较长）
       - cn_dicts/others   # 一些杂项

       # 建议把扩展词库放到下面，有重复词条时，最上面的权重生效
       # - cn_dicts/mydict
       - user-defined
     ```
5. 重新部署`Rime`
