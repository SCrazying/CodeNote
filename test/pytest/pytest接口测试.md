## 接口测试



针对测试引擎现有情况，需要对接口进行测试，主要分为2个方面

- 使用python进行接口测试， 这里使用的计划采用python + pytest + grpc 实现对测试引擎的接口进行测试。
- 使用c++进行接口测试，这里的接口指的是内部接口的接口测试，需要掌握的技术栈是C++、cmake、gtest



## 如何使用python进行接口测试

主要通过一下3个方面进行叙述

### pytest搭建基础的测试框架

主要描述的是如何搭建一个基础的测试框架，满足基本的测试环境，整个测试框架主要基于pytest这个插件进行集成，主要目录如下所示

```bash
-- common					#存放公共的接口，如配置文件，测试数据加载，日志生成
-- config					#存放相关网络配置等
-- protobuf					#存放proto文件
-- testcase					#存放接口测试的用力
-- data						#存放接口文件模拟数据
-- log						#存放接口测试中过程日志
-- report					#存放接口测试的报表
run.py						#整个工程的入口
conan.py					#conan依赖文件，主要用于自动化部署测试环境
```

3.1 安装相关环境

基础环境如下：python3+pytest+Request+Jenkins+Allure

3.1.1 pytest环境安装

```bash
#安装pytest,pytest官网：https://docs.pytest.org/en/latest/index.html
pip install pytest
#检查pytest版本
pytest --version
```

3.1.2 allure环境安装

```bash
#windows安装allure
https://github.com/allure-framework/allure2/releases
#安装allure-pytest依赖
pip install allure-pytest
```

allure报告结构

```c++
1.Overview:整体数据显示。
2.Suites:用例集合,按照套件和类分组的已执行测试的标准结构表示形式。
3.Behaviors：对于行为驱动的方法，此选项卡根据Epic、Feature和Story标记对测试结果进行分组。
4.Categories：“类别”选项卡提供了创建自定义缺陷分类以应用测试结果的方法。
5.Graphs：用图表显示测试数据中收集的不同统计数据，状态分解或严重性和持续时间图。
6.Packages：软件包选项卡表示测试结果的树状布局，按不同的包名分组。
7.Timeline：时间轴选项卡可视化测试执行的回顾，allure适配器收集测试的精确时间，在这个选项卡上，它们相应地按照顺序或并行的时间结构排列。
```

基本用法

```bash
#Allure监听器在测试执行期间会收集结果，只需添加alluredir选项，并选择输出的文件路径即可。
pytest --alluredir=./allure-results
# 执行此命令，将在默认浏览器中显示生成的报告
allure serve ./allure-results
```



第一个测试用例

```python
def func(x):
    return x + 1

def test_answer():
    assert func(3) == 5
        
if __name__ == '__main__':
    pytest.main()
```



### grpc如何快速入门



### pytest集成conan生成测试环境



实战的实例

