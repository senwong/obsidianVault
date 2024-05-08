
# get code

官方文档会下载所有git历史记录，总共100多G，加上--no-history后之后20多G。
```
caffeinate fetch chromium --no-history
```

# buil

展示所有targets
```
gn ls out/Default
```


去除掉target的"//"后就是构建指定的名称，构建content shell
```
autoninja -C out/Default content/shell:content_shell
```

比如构建imageDiff，查找到target `//tools/imagedDiff:imageDiff`，使用构建命令：`autoninja -C out/Default tools/imagedDiff:imageDiff
![[ninja_ls_targets.png]]
# debug
build完成之后out目录下有个Content Shell.app，直接open app不会展示日志
```
open ./out/Default/Content\ Shell.app/  --args --single_process --run-web-tests ~/Downloads/test.html
```

要打开app里的可执行程序
```
open ./out/Default/Content\ Shell.app/Contents/MacOS/Content\ Shell --args --run-web-tests ~/Downloads/test.html
```


## 启动一个content Shell
打开一个terminal tab
```
./out/Default/Content\ Shell.app/Contents/MacOS/Content\ Shell  --no-sandbox --renderer-startup-dialog ~/Downloads/test.html
```
另外开启一个tab，打开lldb
```
lldb -p 35447
```

设置断点
```
b blink::LayoutView::UpdateLayout
```
```
b content::Shell::LoadURL
```
查看线程详情
```
thread list
```
## run_web_tests

参考：https://chromium.googlesource.com/chromium/src/+/HEAD/docs/testing/web_tests.md
使用`strip`命令，去除`content Shell` app的符号，否则启动测试时会启动多个进程。
查看所有的测试用例：`src/third_party/blink/web_tests`。



## Frame

Frame is the base class of LocalFrame and RemoteFrame and should only contain functionality shared between both. In particular, any method related to input, layout, or painting probably belongs on LocalFrame.

## Page
A Page roughly corresponds to a tab or popup window in a browser. It owns a tree of frames (a blink::FrameTree). The root frame is called the main frame. Note that frames can be local or remote to this process.

## Document
A document (https://dom.spec.whatwg.org/#concept-document) is the root node of a tree of DOM nodes, generally resulting from the parsing of a markup (typically, HTML) resource. A document may or may not have a browsing context (https://html.spec.whatwg.org/#browsing-context). A document with a browsing context is created by navigation, and has a non-null domWindow(), GetFrame(), Loader(), etc., and is visible to the user. It will have a valid GetExecutionContext(), which will be equal to domWindow(). If the Document constructor receives a DocumentInit created WithDocumentLoader(), it will have a browsing context. Documents created by all other APIs do not have a browsing context. These Documents still have a valid GetExecutionContext() (i.e., the domWindow() of the Document in which they were created), so they can still access script, but return null for domWindow(), GetFrame() and Loader(). Generally, they should not downcast the ExecutionContext to a LocalDOMWindow and access the properties of the window directly. Finally, unit tests are allowed to create a Document that does not even have a valid GetExecutionContext(). This is a lightweight way to test properties of the Document and the DOM that do not require script.


## 调用栈
content shell 调用栈

```
* thread #1, name = 'CrRendererMain', queue = 'com.apple.main-thread', stop reason = breakpoint 1.1
  * frame #0: 0x00000001b6d0810e libblink_core.dylib`blink::DocumentLoader::CommitNavigation(this=0x0000011100130338) at document_loader.cc:2636:3
    frame #1: 0x00000001b6d713f0 libblink_core.dylib`blink::FrameLoader::CommitDocumentLoader(this=0x000001110012ed60, document_loader=0x0000011100130338, previous_history_item=0x0000000000000000, commit_reason=kInitialization) at frame_loader.cc:1345:21
    frame #2: 0x00000001b6d70612 libblink_core.dylib`blink::FrameLoader::Init(this=0x000001110012ed60, document_token=0x00007ff7b2c954f0, policy_container=nullptr, storage_key=0x00007ff7b2c953e0, document_ukm_source_id=0, creator_base_url=0x00007ff7b2c952d8) at frame_loader.cc:259:3
    frame #3: 0x00000001b59da38a libblink_core.dylib`blink::LocalFrame::Init(this=0x000001110012eb90, opener=0x0000000000000000, document_token=0x00007ff7b2c954f0, policy_container=nullptr, storage_key=0x00007ff7b2c953e0, document_ukm_source_id=0, creator_base_url=0x00007ff7b2c952d8) at local_frame.cc:387:11
    frame #4: 0x00000001b5c71d32 libblink_core.dylib`blink::WebLocalFrameImpl::InitializeCoreFrameInternal(this=0x000001110012ea30, page=0x000001110012df38, owner=0x00000111001a1a18, parent=0x0000000000000000, previous_sibling=0x0000000000000000, insert_type=kInsertLater, name=0x00007ff7b2c95350, window_agent_factory=0x0000011100168268, opener=0x0000000000000000, document_token=0x00007ff7b2c954f0, policy_container=nullptr, storage_key=0x00007ff7b2c953e0, document_ukm_source_id=0, creator_base_url=0x00007ff7b2c952d8, sandbox_flags=kNone) at web_local_frame_impl.cc:2320:11
    frame #5: 0x00000001b5c70286 libblink_core.dylib`blink::WebLocalFrameImpl::InitializeCoreFrame(this=0x000001110012ea30, page=0x000001110012df38, owner=0x00000111001a1a18, parent=0x0000000000000000, previous_sibling=0x0000000000000000, insert_type=kInsertLater, name=0x00007ff7b2c95350, window_agent_factory=0x0000011100168268, opener=0x0000000000000000, document_token=0x00007ff7b2c954f0, policy_container=nullptr, storage_key=0x00007ff7b2c953e0, creator_base_url=0x00007ff7b2c952d8, sandbox_flags=kNone) at web_local_frame_impl.cc:2262:3
    frame #6: 0x00000001b5c6fcf7 libblink_core.dylib`blink::WebLocalFrameImpl::CreateProvisional(client=0x00007fd81801d800, interface_registry=0x0000600000992670, frame_token=0x00007fd817809870, previous_web_frame=0x0000011100168220, frame_policy=0x0000600003bb06a0, name=0x00007ff7b2c95948, web_view=0x00007fd81801f800) at web_local_frame_impl.cc:2134:14
    frame #7: 0x00000001b5c6f8b9 libblink_core.dylib`blink::WebLocalFrame::CreateProvisional(client=0x00007fd81801d800, interface_registry=0x0000600000992670, frame_token=0x00007fd817809870, previous_frame=0x0000011100168220, frame_policy=0x0000600003bb06a0, name=0x00007ff7b2c95948, web_view=0x00007fd81801f800) at web_local_frame_impl.cc:2052:10
    frame #8: 0x0000000159e8ca82 libcontent.dylib`content::RenderFrameImpl::CreateFrame(agent_scheduling_group=0x00007fd8178075a0, frame_token=0x00007fd817809870, routing_id=4, frame_receiver=PendingAssociatedReceiver<content::mojom::Frame> @ 0x00007ff7b2c95c08, browser_interface_broker=PendingRemote<blink::mojom::BrowserInterfaceBroker> @ 0x00007ff7b2c95c20, associated_interface_provider=PendingAssociatedRemote<blink::mojom::AssociatedInterfaceProvider> @ 0x00007ff7b2c95c10, web_view=0x00007fd81801f800, previous_frame_token= Has Value=true , opener_frame_token= Has Value=false , parent_frame_token= Has Value=false , previous_sibling_frame_token= Has Value=false , devtools_frame_token=0x00007fd817809928, tree_scope_type=kDocument, replicated_state=blink::mojom::FrameReplicationStatePtr @ 0x00007ff7b2c95c00, widget_params=content::mojom::CreateFrameWidgetParamsPtr @ 0x00007ff7b2c95bf8, frame_owner_properties=blink::mojom::FrameOwnerPropertiesPtr @ 0x00007ff7b2c95bf0, is_on_initial_empty_document=true, document_token=0x00007fd817809950, policy_container=blink::mojom::PolicyContainerPtr @ 0x00007ff7b2c95be8, is_for_nested_main_frame=false) at render_frame_impl.cc:1775:17
    frame #9: 0x0000000159de614f libcontent.dylib`content::AgentSchedulingGroup::CreateFrame(this=0x00007fd8178075a0, params=content::mojom::CreateFrameParamsPtr @ 0x00007ff7b2c95dc0) at agent_scheduling_group.cc:420:3
    frame #10: 0x0000000155e3aad0 libcontent.dylib`content::mojom::AgentSchedulingGroupStubDispatch::Accept(impl=0x00007fd8178075a0, message=0x00007ff7b2c96650) at agent_scheduling_group.mojom.cc:652:13
    frame #11: 0x0000000159de8d30 libcontent.dylib`content::mojom::AgentSchedulingGroupStub<mojo::RawPtrImplRefTraits<content::mojom::AgentSchedulingGroup>>::Accept(this=0x00007fd8178075f8, message=0x00007ff7b2c96650) at agent_scheduling_group.mojom.h:263:12
    frame #12: 0x000000010e2f8bfc libmojo_public_cpp_bindings.dylib`mojo::InterfaceEndpointClient::HandleValidatedMessage(this=0x00007fd81790dfa0, message=0x00007ff7b2c96650) at interface_endpoint_client.cc:1021:54
    frame #13: 0x000000010e2f8279 libmojo_public_cpp_bindings.dylib`mojo::InterfaceEndpointClient::HandleIncomingMessageThunk::Accept(this=0x00007fd81790e0d8, message=0x00007ff7b2c96650) at interface_endpoint_client.cc:368:18
    frame #14: 0x000000010e316504 libmojo_public_cpp_bindings.dylib`mojo::MessageDispatcher::Accept(this=0x00007fd81790e0e8, message=0x00007ff7b2c96650) at message_dispatcher.cc:43:19
    frame #15: 0x000000010e2fbba6 libmojo_public_cpp_bindings.dylib`mojo::InterfaceEndpointClient::HandleIncomingMessage(this=0x00007fd81790dfa0, message=0x00007ff7b2c96650) at interface_endpoint_client.cc:706:20
    frame #16: 0x00000001139af4fd libipc.dylib`IPC::ChannelAssociatedGroupController::AcceptOnEndpointThread(this=0x00007fd816f08180, message=Message @ 0x00007ff7b2c96650, scoped_urgent_message_notification=ScopedUrgentMessageNotification @ 0x00007ff7b2c96648) at ipc_mojo_bootstrap.cc:1181:24
    frame #17: 0x00000001139b1e63 libipc.dylib`void base::internal::DecayedFunctorTraits<void (IPC::ChannelAssociatedGroupController::*)(mojo::Message, IPC::(anonymous namespace)::ScopedUrgentMessageNotification), IPC::ChannelAssociatedGroupController*&&, mojo::Message&&, IPC::(anonymous namespace)::ScopedUrgentMessageNotification&&>::Invoke<void (IPC::ChannelAssociatedGroupController::*)(mojo::Message, IPC::(anonymous namespace)::ScopedUrgentMessageNotification), scoped_refptr<IPC::ChannelAssociatedGroupController>, mojo::Message, IPC::(anonymous namespace)::ScopedUrgentMessageNotification>(method=(libipc.dylib`IPC::ChannelAssociatedGroupController::AcceptOnEndpointThread(mojo::Message, IPC::(anonymous namespace)::ScopedUrgentMessageNotification) at ipc_mojo_bootstrap.cc:1138), receiver_ptr=0x00006000032b9770, args=0x00006000032b9778, args=0x00006000032b97f0) at bind_internal.h:738:12
    frame #18: 0x00000001139b1d3b libipc.dylib`void base::internal::InvokeHelper<false, base::internal::FunctorTraits<void (IPC::ChannelAssociatedGroupController::*&&)(mojo::Message, IPC::(anonymous namespace)::ScopedUrgentMessageNotification), IPC::ChannelAssociatedGroupController*&&, mojo::Message&&, IPC::(anonymous namespace)::ScopedUrgentMessageNotification&&>, void, 0ul, 1ul, 2ul>::MakeItSo<void (IPC::ChannelAssociatedGroupController::*)(mojo::Message, IPC::(anonymous namespace)::ScopedUrgentMessageNotification), std::__Cr::tuple<scoped_refptr<IPC::ChannelAssociatedGroupController>, mojo::Message, IPC::(anonymous namespace)::ScopedUrgentMessageNotification>>(functor=0x00006000032b9760, bound=size=3) at bind_internal.h:930:12
    frame #19: 0x00000001139b1c9d libipc.dylib`void base::internal::Invoker<base::internal::FunctorTraits<void (IPC::ChannelAssociatedGroupController::*&&)(mojo::Message, IPC::(anonymous namespace)::ScopedUrgentMessageNotification), IPC::ChannelAssociatedGroupController*&&, mojo::Message&&, IPC::(anonymous namespace)::ScopedUrgentMessageNotification&&>, base::internal::BindState<true, true, false, void (IPC::ChannelAssociatedGroupController::*)(mojo::Message, IPC::(anonymous namespace)::ScopedUrgentMessageNotification), scoped_refptr<IPC::ChannelAssociatedGroupController>, mojo::Message, IPC::(anonymous namespace)::ScopedUrgentMessageNotification>, void ()>::RunImpl<void (IPC::ChannelAssociatedGroupController::*)(mojo::Message, IPC::(anonymous namespace)::ScopedUrgentMessageNotification), std::__Cr::tuple<scoped_refptr<IPC::ChannelAssociatedGroupController>, mojo::Message, IPC::(anonymous namespace)::ScopedUrgentMessageNotification>, 0ul, 1ul, 2ul>(functor=0x00006000032b9760, bound=size=3, (null)=std::__Cr::index_sequence<0UL, 1UL, 2UL> @ 0x00007ff7b2c9674f) at bind_internal.h:1067:14
    frame #20: 0x00000001139b1c07 libipc.dylib`base::internal::Invoker<base::internal::FunctorTraits<void (IPC::ChannelAssociatedGroupController::*&&)(mojo::Message, IPC::(anonymous namespace)::ScopedUrgentMessageNotification), IPC::ChannelAssociatedGroupController*&&, mojo::Message&&, IPC::(anonymous namespace)::ScopedUrgentMessageNotification&&>, base::internal::BindState<true, true, false, void (IPC::ChannelAssociatedGroupController::*)(mojo::Message, IPC::(anonymous namespace)::ScopedUrgentMessageNotification), scoped_refptr<IPC::ChannelAssociatedGroupController>, mojo::Message, IPC::(anonymous namespace)::ScopedUrgentMessageNotification>, void ()>::RunOnce(base=0x00006000032b9740) at bind_internal.h:980:12
    frame #21: 0x000000010fba326e libbase.dylib`base::OnceCallback<void ()>::Run(this=0x00007fd81a00fe78) && at callback.h:156:12
    frame #22: 0x000000010fdd73de libbase.dylib`base::TaskAnnotator::RunTaskImpl(this=0x00007fd817905bc8, pending_task=0x00007fd81a00fe00) at task_annotator.cc:202:34
    frame #23: 0x000000010fe54a7e libbase.dylib`void base::TaskAnnotator::RunTask<base::sequence_manager::internal::ThreadControllerWithMessagePumpImpl::DoWorkImpl(base::LazyNow*)::$_3>(this=0x00007fd817905bc8, event_name=(value = "ThreadControllerImpl::RunTask"), pending_task=0x00007fd81a00fe00, args=0x00007ff7b2c969e8) at task_annotator.h:89:5
    frame #24: 0x000000010fe543fc libbase.dylib`base::sequence_manager::internal::ThreadControllerWithMessagePumpImpl::DoWorkImpl(this=0x00007fd8179058f0, continuation_lazy_now=0x00007ff7b2c96cd0) at thread_controller_with_message_pump_impl.cc:473:23
    frame #25: 0x000000010fe5386b libbase.dylib`base::sequence_manager::internal::ThreadControllerWithMessagePumpImpl::DoWork(this=0x00007fd8179058f0) at thread_controller_with_message_pump_impl.cc:338:40
    frame #26: 0x000000010fe546c3 libbase.dylib`non-virtual thunk to base::sequence_manager::internal::ThreadControllerWithMessagePumpImpl::DoWork() at thread_controller_with_message_pump_impl.cc:0
    frame #27: 0x000000010fc4d054 libbase.dylib`base::MessagePumpDefault::Run(this=0x00006000012a9dc0, delegate=0x00007fd8179059d8) at message_pump_default.cc:40:55
    frame #28: 0x000000010fe552a9 libbase.dylib`base::sequence_manager::internal::ThreadControllerWithMessagePumpImpl::Run(this=0x00007fd8179058f0, application_tasks_allowed=true, timeout=TimeDelta @ 0x00007ff7b2c96ed8) at thread_controller_with_message_pump_impl.cc:641:12
    frame #29: 0x000000010fd3c690 libbase.dylib`base::RunLoop::Run(this=0x00007ff7b2c971c8, location=0x00007ff7b2c970a8) at run_loop.cc:134:14
    frame #30: 0x0000000159f278d3 libcontent.dylib`content::RendererMain(parameters=MainFunctionParams @ 0x00007ff7b2c973e8) at renderer_main.cc:367:16
    frame #31: 0x000000015a2faddb libcontent.dylib`content::RunOtherNamedProcessTypeMain(process_type="renderer", main_function_params=MainFunctionParams @ 0x00007ff7b2c97570, delegate=0x00007ff7b2c97b20) at content_main_runner_impl.cc:771:14
    frame #32: 0x000000015a2fc655 libcontent.dylib`content::ContentMainRunnerImpl::Run(this=0x00006000029b8280) at content_main_runner_impl.cc:1146:10
    frame #33: 0x000000015a2f8df8 libcontent.dylib`content::RunContentProcess(params=ContentMainParams @ 0x00007ff7b2c97a38, content_main_runner=0x00006000029b8280) at content_main.cc:333:36
    frame #34: 0x000000015a2f9516 libcontent.dylib`content::ContentMain(params=ContentMainParams @ 0x00007ff7b2c97ab0) at content_main.cc:346:10
    frame #35: 0x000000024d582324
    frame #36: 0x000000010d267b82 Content Shell Helper (Renderer)`main(argc=15, argv=0x00007ff7b2c97ea8) at shell_main_mac.cc:97:8
    frame #37: 0x00007ff80c4ed386 dyld`start + 1942
(lldb)
```

