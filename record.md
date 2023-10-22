**a1**  
chromium 初始，去掉fatal_linker_warnings和treat_warnings_as_errors  
**a2**  
navigation中网页的网络请求，HTTP请求头的设置  
navigation的请求事件执行过程，请求之前注册所有类型事件，利用switch和状态标记触发不同的事件  
tab 专题：  
1. 枚举类 WindowOpenDisposition 给出了tab的打开类型，ui/base/window_open_disposition.h  
2. ChromeBrowserMainParts::PostBrowserStart()  Browser启动后的一些加载工作，此时browser已经初始化完成，但loading content还没有开始，这里会调用AfterStartupTaskUtils::StartMonitoringStartup() 触发相关的observe  
**a3**  
browser_navigator.cc 870行 params->user_gesture 说明是否启用手势 
infobar_utils.cc 中将AddInfoBarsIfNecessary()直接返回(置空)，可以删除tab中的[infobar](https://en.wikipedia.org/wiki/Infobar)     
全屏设置的可疑位置 StartupBrowserCreatorImpl::MaybeToggleFullscreen()，该方法中有一个注释：In kiosk mode, we want to always be fullscreen.     
TabStripModel::ForgetAllOpeners()，也许可以用来隐藏tab页  
tab_strip_model.cc中 996行 set_reset_opener_on_active_tab_change()和隐藏也许有关  
omnibox_view = delegate()->GetOmniboxView()  也许可以用来隐藏omnibox   
ChromeLocationBarModelDelegate::ShouldDisplayURL() 显示URL  
**a4**  
有用的log文件：chrome_debug1021-1.log   
当前的所有断点文件   





**有用的函数和类**  
MaybeToggleFullscreen  
<br/> 
SetShowIcon  
<br/>
ShouldShowWindowIcon, browser_view.cc,847  
<br/> 
tab_menu_model_factory, browser_view.cc 898  
<br/> 
right_aligned_side_panel_separator_, browser_view.cc 965  
infobar_container_, ... 979  
<br/> 
UpgradeNotificationController,... 989  
<br/> 
UpdateFullscreenAllowedFromPolicy 和 CanFullscreen(),... 1013 
BrowserList, browser.cc 571 
<br/>  
ShowPopupMenu, web_contents_impl.h  
<br/> 
HandleMouseEvent(const blink::WebMouseEvent& event), web_contents_impl.h  
<br/> 
HandleKeyboardEvent(const NativeWebKeyboardEvent& event),web_contents_impl.h  
<br/> 
LocationBarModel,tab页中与omnibox相关的类
<br/>  
tab_helprs.cc中挂载了很多相观察者事件，例如下面三个，需要细看：
ChromePasswordManagerClient::CreateForWebContents(web_contents);  
<br/>  
ChromePasswordReuseDetectionManagerClient::CreateForWebContents(web_contents);  
<br/>    
ChromeTranslateClient::CreateForWebContents(web_contents);  
<br/>  
RenderFrameHost->IsActive() 使页面不激活，用户就看不到
<br/>   
WebContents->GetPrimaryMainFrame(),还有一些其它的函数
<br/>  
WebContents->UpdateWebContentsVisibility()，更新可见并通知观察者，如何通知？  
<br/>  
WebContents->NeedToFireBeforeUnloadOrUnloadEvents()，处理事件过程  
<br/>  
WebContents->DispatchBeforeUnload(), Dispatch是什么意思？  
<br/>   
NavigationHandleUserData，猜测会处理各种事件，见：https://source.chromium.org/chromium/chromium/src/+/main:content/public/browser/web_contents.h;l=1415?q=WebContents&ss=chromium    
<br/>  
PAGE_TRANSITION_AUTO_TOPLEVEL 翻译相关的一个枚举值


**有用的材料**  
BroadcastChannel 可以用来跨tab通信，cs.chromium.org中能查到，stackoverflow材料:https://stackoverflow.com/questions/28230845/communication-between-tabs-or-windows    
<br/>  
RenderView respresents a webpage. This object represents a web page. It handles all navigation-related commands to and from the browser process. It derives from RenderWidget which provides painting and input event handling. The RenderView communicates with the browser process via the global (per render process) RenderProcess object. [出处](https://www.chromium.org/developers/design-documents/displaying-a-web-page-in-chrome/)   
<br/>  
[Profile Architecture](https://www.chromium.org/developers/design-documents/profile-architecture/)
