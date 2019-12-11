# Swift

>>> 记录总结Swift在实际使用中遇到的问题。

## Swift和OC混编

OC中要引入Swift类，在OC类中加入 ``#import "your_project-Swift.h"`` （这是在OC项目中新建Swift类时会自动创建的一个隐藏文件），就可以使用Swift定义的类了。
Swift中引入OC类，在 CruiseShipSea-Bridging-Header.h 文件中 import 对应的OC类，就可以使用OC定义的类了。


## Defines Module

## Swift 中的 self

比如 UITableViewCell 上一个 UIButton ，这样写有什么问题？

```
class MyCell: UITableViewCell {

    private let statusBtn: UIButton = {
        let b = UIButton.init()
        b.layer.cornerRadius = 4
        b.layer.masksToBounds = true
        b.titleLabel?.font = UIFont.systemFont(ofSize: 13)
        b.backgroundColor = UIColor(rgb: 0x376FE8)
        b.setTitleColor(.white, for: .normal)
        print(self)
        b.addTarget(self, action: #selector(statusBtnAction), for: .touchUpInside)
        return b
    }()


    override init(style: UITableViewCell.CellStyle, reuseIdentifier: String?) {
        super.init(style: style, reuseIdentifier: reuseIdentifier)
        contentView.addSubview(statusBtn)
    }

    @objc private func statusBtnAction() {
    print("status button action")
    }

}
```
这样写会导致 UIButton 的点击事件不响应，因为在这里 ``self`` 指向的是当前的Function，并不是当前UITableViewCell。正确的写法应该是在 cell 的 init 方法里绑定事件。



## 

[Swift-Tips](https://swifter.tips/)
