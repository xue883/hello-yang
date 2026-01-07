# hello-yang
当前测试脚本项目
#include <jni.h>
#include <android/log.h>
#define LOG_TAG "GameInject"
#define LOGD(...) __android_log_print(ANDROID_LOG_DEBUG, LOG_TAG, __VA_ARGS__)

// 注入入口（Zygisk回调）
extern "C" void zygisk_init(void *handle) {
  LOGD("注入模块已加载");
  // 1. 获取目标游戏包名（需替换为你的游戏包名，如com.tencent.tmgp.pubgmhd）
  const char *target_pkg = "com.example.targetgame";
  
  // 2. 检测当前进程是否为目标游戏
  char pkg[256];
  if (zygisk_get_process_name(handle, pkg, sizeof(pkg)) != 0) return;
  if (strcmp(pkg, target_pkg) != 0) return;
  
  LOGD("成功注入目标游戏：%s", pkg);
  // 3. 自定义注入逻辑（示例：Hook游戏函数）
  JNIEnv *env = zygisk_get_jni_env(handle);
  if (env != nullptr) {
    // 此处可添加Hook游戏数值、修改运行逻辑等代码
    // 需结合游戏逆向分析（如使用IDAPro定位目标函数）
    LOGD("注入逻辑执行完成");
  }
}
