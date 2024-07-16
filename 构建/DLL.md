# DLL
## 导入
- 返回类型不能为字节集。

## 导出
- > 被取地址的子程序的参数必须是基本数据类型，且不能为字节集。
- 返回字节集时返回的是字节集的 content 指针，可以用负偏移获取长度。
  - 子程序会在每次被调用时释放前一次返回的字节集。调用方不该通过 `HeapFree(p - 8)` 自行释放，也不该多线程调用或嵌套调用。

  ```c
  char *__stdcall test()
  {
    char *result; // eax
    char *v1; // [esp-4h] [ebp-10h]

    v1 = (char *)sub_10001107();
    j__krnl_MFree(dword_1018E33C);
    result = v1;
    dword_1018E33C = v1;
    if ( v1 )
      return v1 + 8;
    return result;
  }
  ```