# Loop_Back_Interface
lo, 本地接口

If you are running the command called

    $ifconfig
    
you will see the output of lo, en, p2p and so on.
Lets look at the lo, which means the Loopback || Localhost Interface (本機或稱本地接口)。

The lo interface (lo stands for “loopback”) is a special, virtual network interface in which all traffic (up or down) is automatically sent back to the interface itself without any processing or modification. It is assigned the IP address 127.0.0.1, which is a special IP address that can’t ever be used publicly and always refers to the machine itself.


# The Machine Itself

Think of the loopback interface and the 127.0.0.1 IP as a reflexive pronoun: it always means “this machine itself”. The term “localhost” is often used to mean the same thing in this context.

# for Debugging Usage

The loopback interface is used for debugging and running machine-internal network services. For example, we might have a development version of a web application running and don’t want it available to the internet at large. We could configure it to only send/receive traffic over the loopback interface and then access it at http://127.0.0.1. (這就是我們在身為後端工程師時，經常使用開發測試用的網站網址路徑。)
   
   
