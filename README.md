# rao10086

universal-ssl-check-bypass

# 关闭SELinux强制访问控制机制
setenforce 0
#启动android的脚本
./frida-server
#开启PC的端口转发
adb forward tcp:27042 tcp:27042
adb forward tcp:27043 tcp:27043
#运行PC的服务
frida -R -f 包名 -l 脚本 --no-pause

#univer-ssl-pass.js
/*
   Universal Android SSL Pinning Bypass
   by Mattia Vinci and Maurizio Agazzini
 
adb forward tcp:27042 tcp:27042
adb forward tcp:27043 tcp:27043
    
frida -R -f com.huawei.appmarket -l universal-ssl-check-bypass.js --no-pause
frida -R -f com.huawei.wallet -l universal-ssl-check-bypass.js --no-pause
	
 
    https://techblog.mediaservice.net/2018/11/universal-android-ssl-check-bypass-2/
*/
 
Java.perform(function() {
 
    var array_list = Java.use("java.util.ArrayList");
    var ApiClient = Java.use('com.android.org.conscrypt.TrustManagerImpl');
 
    ApiClient.checkTrustedRecursive.implementation = function(a1, a2, a3, a4, a5, a6) {
         console.log('Bypassing SSL Pinning');
        var k = array_list.$new();
        return k;
    }
 
}, 0);


# bypass-ssl.js
import frida, sys 

jscode = """
'use strict';
Java.perform(function() {
 
    var array_list = Java.use("java.util.ArrayList");
    var ApiClient = Java.use('com.android.org.conscrypt.TrustManagerImpl');
 
    ApiClient.checkTrustedRecursive.implementation = function(a1, a2, a3, a4, a5, a6) {
         console.log('Bypassing SSL Pinning');
        var k = array_list.$new();
        return k;
    }
    
    var SSLContext = Java.use("javax.net.ssl.SSLContext");
    var SSLContext_init = SSLContext.init.overload(
    '[Ljavax.net.ssl.KeyManager;', '[Ljavax.net.ssl.TrustManager;', 'java.security.SecureRandom');

    // Override the init method, specifying our new TrustManager
    SSLContext_init.implementation = function (keyManager, trustManager, secureRandom) {

        send('Overriding SSLContext.init() with the custom TrustManager');

        SSLContext_init.call(this, null, TrustManagers, null);
    };
    
}, 0);
"""

def on_message(message, data): 

    if message['type'] == 'send': 

        print("[*] {0}".format(message['payload'])) 

    else: 

        print(message) 

  


#process = frida.get_usb_device().attach('com.android.mediacenter')  #
#process1 = frida.get_usb_device().attach('com.huawei.hwid')  #
process2 = frida.get_usb_device().attach('com.huawei.appmarket') 

#script = process.create_script(jscode) 
#script1 = process1.create_script(jscode) 
script2 = process2.create_script(jscode)

#script.on('message', on_message) 
#script1.on('message', on_message) 
script2.on('message', on_message) 

#script.load() 
#script1.load() 
script2.load() 




 


sys.stdin.read() 
