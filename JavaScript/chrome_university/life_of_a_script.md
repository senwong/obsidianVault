

一个v8实例称为v8::Isolate
一个Isolate有多个v8::ContextS
每个iframe有一个context
blink通过blink:scriptState访问context

# blink bindings

read more: https://source.chromium.org/chromium/chromium/src/+/main:third_party/blink/renderer/bindings/README.md


# load script

blink获取资源，从网络，缓存，service worker
v8可能缓存编译之后的bytecode在磁盘上

## parser 

pre-parser 和 full-parser，pre-parser不回生成语法树，只做一些语法解析和错误检查，

## interpreter
叫做Ignition的解释器，
输入： AST
ByteodeGenerator walks  AST, generate bytecode
Threaded interpreter, 
- 在table里查找每个bytecode对应的handler，执行
- 有一个register，在stack中。

下面是一段js code和bytecode的对比
![[js_code_and_bytecode.png]]

- 堆中分配的JavaScript对象，不能直接嵌在bytecode中，有一个固定长度的数组叫constant pool，把对象存放到constant pool，在bytecode中使用constant pool的index。

assembler 根据bytecode生成machine code，interpreter运行machine code。
```
void Interpreter::DoAdd(InterpreterAssembler* assembler) {
	
	Node* reg_index = assembler->BytecodeOperandReg(0);
	
	Node* lhs = assembler->LoadRegister(reg_index);
	
	Node* rhs = assembler->GetAccumulator();
	
	Node* result = AddStub::Generate(assembler, lhs, rhs);
	
	assembler->SetAccumulator (result);
	
	assembler->Dispatch();
}

```
