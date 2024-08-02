# Repository_Co-Construction_Note

## 省流

- 推送到dev分支
- 推送时说清楚更改内容

## 代码仓库使用规范

- 分支问题
  - main 为主分支，为确保主分支稳定性， main 分支一般由 develop 分支合并，任何时间都不能直接修改代码。
  - develop 为开发分支，始终保持最新完成以及bug修复后的代码，一般开发的新功能时，feature分支都是基于develop分支下创建的。平时我们提交代码都在 dev 分支。
  - feature 分支是开发新功能时，以develop为基础创建的特性分支。
- 提交规范问题
  - 为了便于追溯，我们提交规范格式为```type(scope): body```参考以下提交规范：
  - type：用于指定commit的类型，约定了feat、fix两个主要type，以及docs、style、build、refactor、revert五个特殊type。

  ```
    # 主要type
    feat:     增加新功能
    fix:      修复bug
    
    # 特殊type
    docs:     只改动了文档相关的内容
    style:    不影响代码含义的改动，例如去掉空格、改变缩进、增删分号
    build:    构造工具的或者外部依赖的改动，例如webpack，npm
    refactor: 代码重构时使用
    revert:   执行git revert打印的message
    
    # 其他type
    test:     添加测试或者修改现有测试
    perf:     提高性能的改动
    ci:       与CI（持续集成服务）有关的改动
    chore:    不修改src或者test的其余修改，例如构建过程或辅助工具的变动
    
    当一次改动包括主要type与特殊type时，统一采用主要type
  ```

  - scope：用于描述改动的范围，格式为项目名/模块名，如果一次commit修改多个模块，建议拆分成多次commit，以便更好追踪和维护。
  - body：填写详细描述，主要描述改动之前的情况及修改动机，对于小的修改不作要求，但是重大需求、更新等必须添加body来作说明。
- 示例：

  ```
    feat(tasks/chassis): add supercapacitor control module
  ```

  ```
    docs(README): add explanation
  ```

  ```
    docs(Xmind): Fixed a bug with the chassis branch
  ```

## 代码编写规范

### 1. 命名法与字符规范

驼峰式命名法
 释义：函数名中的每一个逻辑断点都有一个大写字母来标记。

- 函数名和类型（typedef/struct/union/enum等）的声明使用大驼峰法，即首字母大写。这样可以与HAL库内的函数区分开来。类型名后可以缀加上_t（type）以方便区分
//< 类型的定义，首字母大写
typedef struct
{
        int16_t  Ecd;                       //< 当前编码器返回值
        int16_t  SpeedRPM;                  //< 每分钟所转圈数
        int16_t  TorqueCurrent;             //< 反馈力矩
        uint8_t  Temperature;               //< 温度

        int16_t  LastEcd;                   //< 上一时刻编码器返回值                        
        int16_t  Angle;                     //< 解算后的编码器角度
        int16_t  AngleSpeed;                //< 解算后的编码器角速度        

        int16_t  Target;                    //< 电机的期望参数
        int16_t  Output;                    //< 电机输出值，通常为电流和电压        

}MotorData_t;

- 函数参数表声明和全局变量的定义使用小驼峰法，即首字母小写。
//< 全局变量的定义，首字母小写
CAN_TxBuffer txBuffer0x200forCAN1={
        .Identifier = 0x200
};

- 宏名全大写，逻辑断点以下划线_间隔。
# define K_ECD_TO_ANGLE 0.043945f       //< 角度转换编码器刻度的系数：360/8192
# define ECD_RANGE_FOR_3508 8191        //< 编码器刻度值为0-8191
# define CURRENT_LIMIT_FOR_3508 16384   //< 控制电流范围为正负16384
# define ECD_RANGE_FOR_6020 8191        //< 编码器刻度值为0-8191
# define VOLTAGE_LIMIT_FOR_6020 30000   //< 控制电压范围为正负30000
- 尽量避免名字中出现数字编号，应使用有意义的单词命名，除非逻辑上必须用数字（如CAN1、CAN2等）
- 注意命名的自解释性，变量名通俗且由名词组成，函数名简单且由动宾短语组成。
- 避免硬编码，将程序运行的参数摘出并给与合理的命名。（关于硬编码详见编码规范相关知识）
- 上面并没有对变量名做硬性要求，可能严格遵循某个命名规范对于我们都是不太适用的，我们要做到的就是所有的变量名命名风格统一即可。（关于命名规范详见编码规范相关知识）
字符规范

- 不要用i,j,k,a,b等单一字符给参数命名，这样严重减低代码可读性。如果有循环变量可以用局部变量index，idx等代替。（禁止使用单字节命名变量，但允许使用i,j,k来作为局部循环变量）
- 养成用英文命名的习惯，不要用拼音命名，也不要使用一些无意义的英文缩写，如果变量名过长需要缩写的应在最初定义位置给出注释说明
- 头文件与源文件中函数体之间、函数和类型的声明之间应当间隔两到三个空行，不多于三个空行，更不要乱加空行。
- 在代码发行版里不允许出现被注释掉的代码。
- 定义的变量无论多简单 最好都写上注释（不然就会出现步兵发弹代码里一堆未参与控制的变量，最后变成屎山的情况），无用的变量及时删除。
- 在计算符号的前后应当添加空格，并排的等号应该跟前后行对齐。
//< 计算符号和参与计算的变量用空格隔开
txBuffer0x200forCAN1.Data[(id - 0x201) * 2]     =  motor_data.Output >> 8;
txBuffer0x200forCAN1.Data[(id - 0x201) * 2 + 1] =  motor_data.Output &  0xff;

- 函数的参数表在填写时，应该在,后面添加一个空格。
void MecanumChassisSetFollowPID(Chassis* chassis, BasePID_Object followPID)

- 在定义或声明指针变量时，*应该紧跟变量类型，并在*后加一个空格。
void MotorFillData(Motor* motor, int16_t output);

2. 头文件格式

头文件作用：

- C语言里，每个源文件是一个模块，头文件为使用该模块的用户提供接口。接口指一个功能模块暴露给其他模块用以访问具体功能的方法。用户只需包含相应的头文件就可使用该头文件中暴露的接口。

头文件组织原则：

- 头文件中不应包含本地数据，以降低模块间耦合度 （迫于无奈）
- 只有源文件自己使用的类型、宏定义和变量、函数声明，不应出现在头文件里。作用域限于单文件的私有变量和函数应声明为static，以防止外部调用。（将私有类型置于源文件中，会提高聚合度，并减少不必要的格式外漏）
- 头文件内不允许定义变量和函数，只能有宏、类型(typedef/struct/union/enum等)及变量和函数的声明。

头文件包含原则：

- 特殊情况下可extern基本类型的全局变量，源文件通过包含该头文件访问全局变量。但头文件内不应extern自定义类型(如结构体)的全局变量，否则将迫使本不需要访问该变量的源文件包含自定义类型所在头文件。（迫于无奈）
- 尽量在源文件中包含头文件，而非在头文件中。且源文件仅包含所需的头文件。（有些库基础库必须包含在头文件里）

头文件格式示例：
# ifndef  *MOTOR_H*      // header guard
# define  *MOTOR_H*      // 必须确保header guard宏名永不重名

//.h文件头部
# ifdef  __cplusplus     // C++与C语言混编头文件
extern "C" {
# endif

//<宏定义声明>
# define K_ECD_TO_ANGLE 0.043945f                    //< 角度转换编码器刻度的系数为360/8192
# define ECD_RANGE_FOR_3508 8191                     //< 编码器刻度值为0-8191

//<类型(typedef/struct/union/enum等)声明>
typedef struct
{
        uint8_t  CanNumber;                         //< 电机所使用的CAN端口号
        uint16_t CanId;                             //< 电机ID
        uint8_t  MotorType;                         //< 电机类型
        uint16_t EcdOffset;                         //< 电机初始零点
        uint16_t EcdFullRange;                      //< 编码器量程
        int16_t  CurrentLimit;                      //< 电调能承受的最大电流
}MotorParam;

//<函数声明>
void MotorFillData(Motor* motor, int16_t output);

//<引用变量声明>
extern RC_Ctrl rc_Ctrl;

//.h文件尾部
# ifdef  __cplusplus
}
# endif

# endif

3. 注释风格
Doxygen注释风格

咱们仅借用这个注释格式，不使用Doxygen这个软件工具。

- 函数注释在源文件中简明扼要；
/**
  - @brief 简要注释函数功能 Brief Description.
  */
当你使用VSCode等代码编辑器时，可以使用一些插件来Generate Doxygen Comments，这会极大提高你写注释的效率。
- 在头文件中全面详细。
/**
  - @brief          电机初始化
  - @note            GM6020电机ID为001时，反馈报文为0x205,
  -                         电机ID为111时（7），反馈报文ID为0x20B
  - @param[in]  motor        电机数据结构体
  - @param[in]  ecd_Offset   编码器初始值
  - @retval     None
  */

//< 这是注释
// 这是注释
