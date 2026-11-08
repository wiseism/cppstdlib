# 修复记录：CI shuffle1.cpp 编译失败（2026-11-08）

## 失败信息
- 仓库：wiseism/cppstdlib
- Workflow：CMake on a single platform
- 失败 run：[42182128348](https://github.com/wiseism/cppstdlib/actions/runs/42182128348)（2026-11-06）
- 失败步骤：Configure/Build（g++ 编译 algo/shuffle1.cpp）

## 错误日志摘录
```
algo/shuffle1.cpp:32:5: error: 'default_random_engine' was not declared in this scope
algo/shuffle1.cpp:34:14: error: 'dre' was not declared in this scope
gmake[2]: *** [CMakeFiles/shuffle1.dir/build.make:79: CMakeFiles/shuffle1.dir/algo/shuffle1.cpp.o] Error 1
##[error]Process completed with exit code 2.
```

## 原因分析
`default_random_engine` 位于 `<random>` 头文件中，原代码缺少 `#include <random>`。
新版本 g++（Ubuntu 24.04 runner）不再通过其他标准头暴露这些符号，导致编译失败。

## 修复内容
- `algo/shuffle1.cpp`：增加 `#include <random>`（提交 9d0b64c，随本文档一并 git 追踪推送）。
- `CMakeLists.txt` 已有 `set(CMAKE_CXX_STANDARD 11)`，无需调整。

## 验证
- 推送后触发新 run，结果见下方补充记录：

| run id | 结果 |
| --- | --- |
| 待推送触发 | 待填 |
