# Shell脚本批量格式化文件名

>>> 利用 Shell 批量把文件夹下的图片从时间戳格式的名字改成年月日时分秒格式的名字。

file.sh文件内容：

```
# for循环
# ls 筛选查询。-d：只列出目录条目  *匹配一个或多个字符
# 将秒级的时间戳转换成日期字符串 date -r 1548205689 '+%Y-%m-%d %H:%M:%S'
# 参考：
# 0 https://www.runoob.com/linux/linux-shell.html
# 1 https://blog.csdn.net/taiyang1987912/article/details/38929069
# 2 http://noahsnail.com/2016/11/12/2016-11-12-ls%E5%91%BD%E4%BB%A4%E4%BB%8B%E7%BB%8D/
# 3 https://man.linuxde.net/shell-script/shell-7
# 4 http://niliu.me/articles/1450.html#more-1450
# 5 https://stackoverflow.com/questions/806906/how-do-i-test-if-a-variable-is-a-number-in-bash/806923
# 6 https://stackoverflow.com/questions/12187859/create-new-file-but-add-number-if-filename-already-exists-in-bash

# shell函数的定义与使用
isNumberStr() {
    # echo "第一个参数为 $1 !"
    # shell使用正则表达式验证是否数字
    re='^[0-9]+$'
    if ! [[ $1 =~ $re ]]; then
        echo "Error: $1 is not a number."
        return 0
    else
        # echo "Valid number."
        return 1
    fi
}

# format 2018-02-01 01:01:01
tryFormat() {
    timestamp=$1
    suffix=$2
    index=$3

    # 调用函数
    isNumberStr $timestamp
    # 获取函数返回值
    isNum=$?
    # 条件表达式要放在方括号之间，并且要有空格
    if [ $isNum -eq 1 ]; then
        len=${#timestamp}
        msLen=13
        sLen=10
        if [ $len -eq $msLen ]; then
            timestamp=${timestamp:0:10}
        fi

        # shell表达式，注意是反引号``不是单引号''
        # 注意系统中文件名中不能包含冒号（:）等特殊符号，这里如果写成%H:%M:%S，写入的时候会自动把冒号变为/。
        ds=`date -r $timestamp '+%Y-%m-%d %H-%M-%S'`
        regex='[0-9]{4}-[0-9]{2}-[0-9]{2} [0-9]{2}-[0-9]{2}-[0-9]{2}'
        if [[ $ds =~ $regex ]]; then
            # echo "Is a timestamp"
            # 重命名
            oldFile=$1.$suffix
            newFile=$ds.$suffix
            # 因为新的文件名中有空格
            # mv操作加-i遇到同名的询问要不要覆盖
            if [[ -e "$newFile" ]]; then
                echo "文件名重复了"
                # 文件名重复的时候这里直接在文件名后面加上当前index
                newFile=$ds.$index.$suffix
            fi

            echo "$newFile"
            mv -i $oldFile "$newFile"

        else
        echo "$timestamp is not a valide timestamp"
        fi
    fi
}

index=0
for file in $( ls -d {*.png,*.jpg,*.jpeg,*gif,*.PNG,*.JPG,*.JPEG,*GIF} )
do
    let index++
    echo "file: $file"
    # 截取文件名 注意定义变量=前后不能有空格
    # ${VAR%.* }含义：从$VAR中删除位于 % 右侧的通配符左右匹配的字符串，通配符从右向左进行匹配。% 非贪婪 %% 贪婪。
    fileName=${file%.*}
    # echo $fileName
    # 字符串长度
    # fileNameLen=${#fileName}
    # echo "fileName length= $fileNameLen"
    # 子串
    # echo ${fileName:0:3}
    # ${VAR#*.} 含义：从 $VAR 中删除位于 # 右侧的通配符所匹配的字符串，通配符是左向右进行匹配。# 非贪婪 ## 贪婪。
    suffix=${file#*.}
    # echo "suffix= $suffix"

    # 调用函数 传参
    tryFormat $fileName $suffix $index

    echo "---------------------------------------"
done

```

使用方法：把该文件拷贝到要批量处理的图片文件夹下，然后在终端中执行该shell文件。
