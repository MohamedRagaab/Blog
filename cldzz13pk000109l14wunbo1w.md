---
title: "NodeJS Internal Structure"
seoTitle: "NodeJS Internal Structure"
seoDescription: "NodeJS is a free and open-source app that enables you to run JavaScript outside the browser and also enables it to communicate with the operating system"
datePublished: Sat Feb 11 2023 13:06:10 GMT+0000 (Coordinated Universal Time)
cuid: cldzz13pk000109l14wunbo1w
slug: nodejs-internal-structure
cover: https://cdn.hashnode.com/res/hashnode/image/upload/v1676120916845/4c08a756-7801-4e4f-bee1-a77163770b6c.png
ogImage: https://cdn.hashnode.com/res/hashnode/image/upload/v1676120673440/0e847308-e5ff-4064-8104-b7fd61b4477a.png
tags: nodejs, advance-javascript

---

#### NodeJS Internals and dependencies

A couple of weeks ago, I started a new job as a software engineer at BOSTA company. And I specialized in backend engineering. I found myself writing JavaScript with NodeJS for the first time I had just watched a crash course for NodeJS before. but I don't know how it internally works! how can JavaScript be a server-side language and communicate with the operating system. so I decided to go deeper and learn the advanced topics and know what is going on behind the scene.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1675890545942/57e71c6d-516c-4859-954d-47ddf4f51256.png align="center")

* NodeJS is a free and open-source app that enables you to **run JavaScript outside the browser** and also enables it to **communicate with the operating system** and to work as a server-side language. And it has also dependencies like **V8** and **Libuv**.
    

##### **What is V8 dependency?**

* It is an open-source JavaScript engine created by Google.
    
* The purpose of this project is to **execute JavaScript code outside the browser**. and that's what we do when we run our JavaScript code through the terminal.
    

##### **What is Libuv dependency?**

* It is a C++ open-source project, **that gives NodeJS access to the operating system, file system, and networking** and also handles so aspects of concurrency as well.
    

#### Why NodeJS?

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1675891822522/0be13633-e6fa-4880-987f-9fb9f329c030.png align="center")

* You are probably thinking that we have a dependency here that accesses the operating system, file system and networking. And another dependency that accesses the operating system. So what is the purpose of NodeJS? Why do we just use these dependencies directly?
    
* First, you need to understand that all these libraries or dependencies are not 100% JavaScript code but some of these libraries are completely written in C++.
    
* Even the main dependencies such as Libuv are 100% C++ code and V8 is 70% C++ and only 30% JavaScript code. **and NodeJS layer enables you to write only JavaScript code and just have it works.**
    
* NodeJS gives you a nice **interface layer** in your application and it has a lot of modules such as HTTP, FS, Crypto and Path that are implemented inside the Libuv dependency.
    

#### NodeJS Binding Process

* If you go to the Node repo on GitHub, you will see all the project files and folders. but we can focus on these two folders at this moment:
    
* lib folder -&gt; It is the JavaScript side of the NodeJS
    
* src folder -&gt; It is the C++ side of the NodeJS
    
    ![](https://cdn.hashnode.com/res/hashnode/image/upload/v1676038857608/1e153485-b80c-42fd-88d9-3d7da3b4c539.png align="center")
    
* You are probably wondering how C++ communicates with JavaScript. Well, let's take a simple example to understand this process.
    
* If you already are a NodeJS developer you should use this function (**readFile()**) before in the fs module. And if you go to the implementation of this function in the lib folder you will see this code:
    

```javascript
async function readFile(path, options) {
 options = getOptions(options, { flag: 'r' });
 const flag = options.flag || 'r';

 if (path instanceof FileHandle)
   return readFileHandle(path, options);

 checkAborted(options.signal);

 const fd = await open(path, flag, 0o666);
 return handleFdClose(readFileHandle(fd, options), fd.close);
}
```

* **readFile()** function calls\*\* another JavaScript function (**readFileHandle()**) and if you go to the implementation of this function you will see this code:
    

```javascript
async function readFileHandle(filehandle, options) {
...
const bytesRead = (await binding.read(filehandle.fd, buffer, offset,
                                          length, -1, kUsePromises)) ?? 0;
    totalRead += bytesRead;
...

}
```

* As you can see here in the above implementation; It calls **binding.read()** which is the real implementation in C++ that enables NodeJS to read a file system. but I think you are asking now where is the C++. Well, here the V8's role comes which is converting the JavaScript values to C++ values. If you go to the node\_file.cc in the src folder you will see this code:
    
    ```cpp
        FS_TYPE_TO_NAME(CLOSE, "close")
        FS_TYPE_TO_NAME(READ, "read")
        FS_TYPE_TO_NAME(WRITE, "write")
    ```
    

Which is something called **Internal Binding**. And it is a bridge between the JavaScript side and the C++ side. Now If you go to the implementation of the Read() function in this file you will see the actual C++ code that communicate with the operating system.

```cpp
static void Read(const FunctionCallbackInfo<Value>& args) {
  Environment* env = Environment::GetCurrent(args);

  const int argc = args.Length();
  CHECK_GE(argc, 5);

  CHECK(args[0]->IsInt32());
  const int fd = args[0].As<Int32>()->Value();

  CHECK(Buffer::HasInstance(args[1]));
  Local<Object> buffer_obj = args[1].As<Object>();
  char* buffer_data = Buffer::Data(buffer_obj);
  size_t buffer_length = Buffer::Length(buffer_obj);

  CHECK(IsSafeJsInt(args[2]));
  const int64_t off_64 = args[2].As<Integer>()->Value();
  CHECK_GE(off_64, 0);
  CHECK_LT(static_cast<uint64_t>(off_64), buffer_length);
  const size_t off = static_cast<size_t>(off_64);

  CHECK(args[3]->IsInt32());
  const size_t len = static_cast<size_t>(args[3].As<Int32>()->Value());
  CHECK(Buffer::IsWithinBounds(off, len, buffer_length));

  CHECK(IsSafeJsInt(args[4]) || args[4]->IsBigInt());
  const int64_t pos = args[4]->IsNumber() ?
                      args[4].As<Integer>()->Value() :
                      args[4].As<BigInt>()->Int64Value();

  char* buf = buffer_data + off;
  uv_buf_t uvbuf = uv_buf_init(buf, len);

  FSReqBase* req_wrap_async = GetReqWrap(args, 5);
  if (req_wrap_async != nullptr) {  // read(fd, buffer, offset, len, pos, req)
    FS_ASYNC_TRACE_BEGIN0(UV_FS_READ, req_wrap_async)
    AsyncCall(env, req_wrap_async, args, "read", UTF8, AfterInteger,
              uv_fs_read, fd, &uvbuf, 1, pos);
  } else {  // read(fd, buffer, offset, len, pos, undefined, ctx)
    CHECK_EQ(argc, 7);
    FSReqWrapSync req_wrap_sync;
    FS_SYNC_TRACE_BEGIN(read);
    const int bytesRead = SyncCall(env, args[6], &req_wrap_sync, "read",
                                   uv_fs_read, fd, &uvbuf, 1, pos);
    FS_SYNC_TRACE_END(read, "bytesRead", bytesRead);
    args.GetReturnValue().Set(bytesRead);
  }
}
```

* Now you have a bit of knowledge of the Internal Binding concept and how JavaScript can be a server-side language with the help of NodeJS.