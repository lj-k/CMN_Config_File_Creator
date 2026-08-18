# CMN700 Configuration Generator

**Version: 0.20** | Author: kong@GLM | Email: lj.k@outlook.com

基于 ARM CMN700 R3P0 (Kampos) 的 Mesh 拓扑配置可视化生成工具。通过图形化界面拖拽设备、配置参数、运行 DRC 校验，一键生成可执行的 Ruby 脚本。

项目地址：https://lj-k.github.io/CMN_Config_File_Creator/

## 功能概览

### 图形化拓扑编辑

- 支持 1x1 ~ 12x12 Mesh 拓扑，可视化拖拽 MXP 节点上的设备
- 支持全部 16 种设备类型：HNF, HNI, HND, HNT, HNP, HNV, RNFEESAM, RNFBESAM, RNFCESAM, RNFDESAM, RND, RNI, SNFE, SNFD, SNFC, SBSX, CCG, CXRH
- 支持 CAL (Component Aggregation Layer) 2-way / 4-way / Bypass / RC 类型
- 设备间支持 DCS, CDB, ADB, CXSDB, AXU, A4S 等接口的添加与删除
- MXP 节点复制/粘贴 (Ctrl+C/V)
- 右键菜单操作 + 中键快捷删除

### 参数配置

- Global / MXP / Device 三级参数体系，覆盖 ARM `por_params.yml` 全部可配置参数
- 参数枚举下拉选择、实时校验、默认值灰色提示
- 一键 Reset to Default
- 静态参数 (static mode) 只读保护
- DSA 模式支持

### DRC 设计规则校验

内置 40+ 条 DRC 规则，与 ARM 官方 DRC 脚本逐条对齐：

| 规则类别 | 规则数量 | 来源 |
|----------|---------|------|
| DEV (设备数量/位置) | 14 | feature_drc.rb, hni_drc.rb |
| MXP (端口/设备约束) | 9 | mxp_drc.rb, gt2_device_port_drc.rb |
| CCG (CCG 专属) | 4 | cxg_drc.rb |
| SN (SNF/SBSX) | 3 | sn_drc.rb |
| RNI (RNF 需求) | 2 | rni_drc.rb |
| KAMPOS (系统级) | 3 | kampos_drc.rb |
| PARAM (参数约束) | 7 | parameters_drc.rb |
| AXU/A4S/DCS (接口) | 6 | axu_drc.rb, a4s_drc.rb, credit_slice_drc.rb |

### Ruby 脚本生成

- 生成符合 ARM `cmn_create` API 规范的 Ruby 脚本
- 包含设备创建、CAL 配置、接口参数、域分配、全局/设备参数
- 下载文件名格式：`create_cmn700_MxN[_name].rb`
- ADB 参数按设备类型区分 (RND 生成 CR/AC 通道，其余仅 5 通道)

### SVG 导出

- 一键导出拓扑图为 SVG 文件
- 包含完整的 MXP、设备、CAL、接口图形元素
- 支持铺满页面显示

## 项目结构

```
CMNCreate/
├── index.html                          # 主应用 (单文件 HTML+CSS+JS)
├── data/
│   └── kampos_r3_featureset.json       # 设备/CAL/参数/DRC 规则定义
├── docs/
│   ├── dev-guide-v0.20.html            # 最新开发文档
│   ├── dev-guide-v0.19.html
│   └── ...                              # 历史版本文档
├── README.md                           # 本文件
└── .gitignore
```

## 快速开始

1. 用浏览器打开 `index.html` 即可使用，无需安装任何依赖
2. 在顶部工具栏设置 Mesh 尺寸 (如 4x4)
3. 从左侧设备面板拖拽设备到 MXP 节点的端口上
4. 右键点击设备可打开上下文菜单，添加/删除接口、删除设备
5. 在右侧面板配置 Global / MXP / Device 参数
6. 点击 "Run DRC" 校验配置合法性
7. 点击 "Download .rb" 下载生成的 Ruby 脚本

## 技术栈

- 纯 HTML + CSS + JavaScript (单文件，零依赖)
- SVG 图形渲染
- 基于 ARM CMN700 R3P0 (PL624-DE-99100-r3p0-00eac0) 官方文档与 DRC 代码

## 版本历史

| 版本 | 主要变更 |
|------|---------|
| v0.20 | 右侧参数面板新增 "Parameters" 分隔标题 |
| v0.19 | 下载 .rb 加 create_ 前缀；删除功能重构(右键菜单+中键)；Interface 开关修复；ADB 参数错误修复(CR/AC 仅 RND) |
| v0.18 | MXP_006 DRC 修复；SF_MAX_RNF_PER_CLUSTER 默认值修正；DSA_EN 静态参数 |
| v0.17 | 参数默认值/名称修正；gt2_device_port_drc.rb DRC 规则补全 |
| v0.16 | SVG 导出修复；NUM_REMOTE_RNF 默认值修正；RNSAM 参数范围修正 |
| v0.15 | 空参数设备显示；Reset 按钮；三色参数规则；SVG 导出；铺满页面 |
| v0.14 | feature_drc.rb 全覆盖；DSA 模式；设备类型校验 |
| v0.13 | 参数枚举下拉；实时校验；统一命名 |
| v0.12 | Ruby 生成域分配；MXP 域编辑器 |
| v0.11 | CXRH/A4S 支持矩阵修复；DN/CCG/SN/参数 DRC 规则 |
| v0.10 | MXP 复制/粘贴；参数工具提示 |
| v0.09 | DEV_010 (min 1 HND) 约束 |

## 许可

私有项目，未授权不得分发。

## 联系

- Author: kong@GLM
- Email: lj.k@outlook.com
