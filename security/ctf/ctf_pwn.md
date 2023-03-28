## 可执行文件执行的流程  

terminal --> kernel --> ld.so --> ELF中的_start --> libc.so中__libc_start_main() --> ELF中(.init .init_array) --> ELF中main() --> ELF中(.fini .fini_array) --> syscall exit()

## Payload构造

32位：padding + 当前函数返回地址（记为函数A） + 函数A的返回地址（记为函数B） + 函数A的参数
注意：函数A的返回地址比函数A的参数后入栈
64位：padding + 当前函数返回地址 + pop_rdi_addr + 第一个参数的地址 + 希望调用的函数