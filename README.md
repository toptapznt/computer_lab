### COMPUTER LAB AS A SERVICE USING CONTAINER AND CONTAINER ORCHESTRATION TECHNOLOGY 💻
<br>

```bash
.
├─ 📁 demo
│   └─ 63-2_CS402_14_ppr-r2_demo.mp4
├─ 📁 doc
│   ├─ 63-2_CS402_14_ppr-r2.pdf
│   ├─ 63-2_CS402_14_ppr-r2_abstract_en.txt
│   └─ 63-2_CS402_14_ppr-r2_abstract_th.txt
├─ 📁 exam
│   ├─ 63-2_CS402_14_ppr-r2_exam.mp4
│   └─ 63-2_CS402_14_ppr-r2_exam.png
├─ 📁 src
│   ├─ 📁 client
│   ├─ 📁 script
│   └─ 📁 server
└─ README.md
```

<br>
# **About The Project**


# **Getting Start**

จะแบ่งเป็น 2 ส่วนคือ 
* Setup Kubernetes Cluster 
* Setup Web Application
---

## **Setup Kubernetes Cluster**

1. ใน XMPP Server Node ให้ run script ที่ชื่อว่า "xmppserver-setup.sh"

2. เข้าไปตั้งค่า XMPP Server ที่ http://\<XMPP-Server-IPaddress>:9090
โดยวิธีการตั้งค่าอยู่ในนี้ [Config Openfire](https://edgevpn.io/openfiredocker/) และทำการสร้าง User สำหรับ Master Node และ Worker Node ตามจำนวนที่ต้องการและ สร้าง Group ซึ่งตั้งค่า **Contact List** เป็น **All Users** โดยใน Group มีสมาชิกเพียง 1 Node คือ Master Node

3. ในทุกๆ Node(ทั้ง Master และ Worker) ลง Software EdgeVPN.io โดยใช้ Script ที่ชื่อว่า "setupedgevpn.io.sh" โดยใช้ parameter 4 ตัวคือ 
    * Host Address ของ XMPP Server 
    * E-mail ของ User ใน XMPP Server
    * Password ของ User ใน XMPP Server
    * Virtual IP Address ที่ต้องการใช้ติดต่อกัน (เช่น 10.10.10.01)
    
4. ใน Master Node ใช้ Script ที่ชื่อว่า "shellformasternode.sh" และเข้าไปแก้ไขไฟล์
    ```
    /etc/systemd/system/kubelet.service.d/10-kubeadm.conf 
    ```
    โดยทำการเพิ่ม --node-ip ใน KUBELET_CONFIG_ARGS โดยใช้ Private IP Address ของเครื่องที่ใช้รัน Script ในขั้นตอนที่ 3
    ```
    Environment="KUBELET_CONFIG_ARGS=--config=/var/lib/kubelet/config.yaml --node-ip=<Virtual-IP-Address>"
    ```
    และรันคำสั่ง
    ```
    sudo systemctl daemon-reload
    sudo systemctl restart kubelet
    sudo kubeadm init --pod-network-cidr=10.244.0.0/16 --apiserver-advertise-address=<Virtual IP Address ของ Master Node>
    ```
    จากนั้นจะได้คำสั่งและ Token สำหรับการ Join Cluster มาเพื่อเอาไปให้ Worker Node ใช้ในการ Join Cluster
    ตัวอย่างเช่น
    ```
    sudo kubeadm join 10.10.10.10:6443 --token yxigqc.vwmi19vbiedklgp7     --discovery-token-ca-cert-hash sha256:590b6698140222b480549e0c7f949ecb4db96c961f388a6377765efe8fde35f1

    ```
    แล้วรันคำสั่ง
    ```
    mkdir -p $HOME/.kube
    sudo cp -i /etc/kubernetes/admin.conf $HOME/.kube/config
    sudo chown $(id -u):$(id -g) $HOME/.kube/config
    ```
    ต่อมาคือการ Deploy Pod Network บน Cluster เพื่อทำให้เครื่องภายใน Cluster สามารถติดต่อกันได้โดยใช้คำสั่ง
    ```
    sudo kubectl apply -f https://raw.githubusercontent.com/coreos/flannel/master/Documentation/kube-flannel.yml
    ```

5. ในส่วนของ Worker Node ใช้ Script ที่ชื่อว่า "setupk8s.sh" โดยใช้ parameter 1 ตัวคือ 
    * HostName ที่ต้องการตั้งให้ Worker Node 

    และเข้าไปแก้ไขไฟล์

    ```
    /etc/systemd/system/kubelet.service.d/10-kubeadm.conf 
    ``` 
    โดยทำการเพิ่ม --node-ip ใน KUBELET_CONFIG_ARGS โดยใช้ Private IP Address ของเครื่องที่ใช้รัน Script ในขั้นตอนที่ 3
    ```
    Environment="KUBELET_CONFIG_ARGS=--config=/var/lib/kubelet/config.yaml --node-ip=<Virtual-IP-Address>"
    ```
    และใช้คำสั่งที่ใช้ในการ join cluster ที่ได้มาจาก Master Node ในขั้นตอนที่ 4 

    ```   
    sudo kubeadm join 10.10.10.10:6443 --token yxigqc.vwmi19vbiedklgp7     --discovery-token-ca-cert-hash sha256:590b6698140222b480549e0c7f949ecb4db96c961f388a6377765efe8fde35f1
    ```   
สามารถเช็คสถานะของเครื่องภายใน Cluster โดยใน Master Node ใช้คำสั่ง 
```
kubectl get node
```     
สามารถสร้าง Token ที่ใช้ในการ Join Cluster ใหม่ได้โดยใน Master Node ใช้คำสั่ง 
```
kubeadm token create --print-join-command
``` 

---

## **Setup Web Application**

### ติดตั้ง Software ที่จำเป็น
* node version 14.15.4
* XAMPP 

### **ขั้นตอนการ Setup Web Application**
1.  สร้าง Database ใน http://\<Master-Node-iP>/phpmyadmin
    ที่มีชื่อว่า computer_lab และ Collation เป็น utf8_general_ci
    และทำการสร้าง User ใน table User เพื่อใช้ในการเข้าไปจัดการข้อมูลด้านใน
    ```
    username = admin
    email    = admin@vl.edu
    password = $2a$08$cSYX.1gx6KbKUjy8KAdYeOyXENEE0v05HNZdddM5GXN0khZj.WNOm
    name     = admin
    role     = admin
    ```
    โดย pasword ได้มากจากการ encode text "12345678"
2.  clone Code ของ Web Application ลงมาและเข้าไป ใช้คำสั่ง 
    ```
    npm install 
    ``` 
    ในโฟลเดอร์ Client และ Server 
    และเข้าไปแก้ไขข้อมูลใน
    ``` 
    client/src/Config/IpConfig.js
    ``` 
    โดยเปลี่ยนแปลง SERVER_IP ให้เป็น IP ของ Master Node
    ``` 
    module.exports = {
          SERVER_IP: "<Master-Node-IP>"
    };
    ``` 
    และสร้าง Directory ที่ชื่อว่า yaml และ servicelist ในโฟลเดอร์ server

3. ใช้คำสั่ง ```npm start ``` ในฝั่ง Client และ Server  
