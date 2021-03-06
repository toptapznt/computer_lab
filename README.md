### COMPUTER LAB AS A SERVICE USING CONTAINER AND CONTAINER ORCHESTRATION TECHNOLOGY
### การให้บริการห้องปฏิบัติการคอมพิวเตอร์ด้วยเทคโนโลยีคอนเทนเนอร์เเละการจัดการคอนเทนเนอร์

---

<br>

### Directory Tree (63-2_CS402_14_ppr-r2)

```bash
.
├─ 📁 demo
│   └─ 63-2_CS402_14_ppr-r2_demo.mp4
├─ 📁 doc
│   ├─ 63-2_CS402_14_ppr-r2.pdf
│   ├─ 63-2_CS402_14_ppr-r2_abstract_en.txt
│   ├─ 63-2_CS402_14_ppr-r2_abstract_th.txt
│   ├─ 63-2_CS402_14_ppr-r2_user_manual.pdf
│   └─ 63-2_CS402_14_ppr-r2_setup.pdf
├─ 📁 exam
│   ├─ 63-2_CS402_14_ppr-r2_exam.mp4
│   └─ 63-2_CS402_14_ppr-r2_exam.png
├─ 📁 src
│   ├─ 📁 client
│   ├─ 📁 script
│   └─ 📁 server
├─ 📁 etc
│   └─ 63-2_CS402_14_ppr-r2_demo_workflow.mp4
└─ README.md
```
<br>

# **About The Project**
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; ปัจจุบันการให้บริการห้องปฏิบัติการคอมพิวเตอร์ มักอาศัยการติดตั้งซอฟต์แวร์ที่อยู่เฉพาะเครื่องภายในห้องปฏิบัติการหนึ่งๆ เท่านั้น ซึ่งนอกจากจะเป็นการใช้ทรัพยากรของเครื่องมากเกินความจำเป็น ยังทำให้เกิดข้อจำกัดในการใช้งานซอฟต์แวร์ดังกล่าว เนื่องจากจำเป็นต้องเดินทางมาใช้งานที่ห้องปฏิบัติการซึ่งมีซอฟต์แวร์ติดตั้งไว้เท่านั้น หากเกิดปัญหาที่ทำให้ไม่สามารถเข้าไปใช้งานห้องปฏิบัติการดังกล่าวได้ ก็จะส่งผลให้ไม่สามารถใช้งานเครื่องคอมพิวเตอร์ภายในห้องปฏิบัติการเช่นกัน นอกจากนี้การจัดการห้องปฏิบัติการคอมพิวเตอร์ในปัจจุบัน การดำเนินงานมีความซ้ำซ้อนและใช้เวลาในการดำเนินการค่อนข้างนาน <br>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; ผู้พัฒนาระบบจึงพัฒนาบริการด้วยเทคโนโลยีคอนเทนเนอร์และการจัดการคอนเทนเนอร์ขึ้น เพื่อทำให้การจัดการห้องปฏิบัติการคอมพิวเตอร์มีประสิทธิภาพมากขึ้น โดยระบบที่พัฒนาขึ้นอาศัยการสร้างคลัสเตอร์ระหว่างเครื่องควบคุมและเครื่องคอมพิวเตอร์ในห้องแลป หรือ ไคลแอนต์ (Client) โดยใช้ [Kubernetes](https://kubernetes.io/docs/concepts/overview/what-is-kubernetes/) และใช้ [ดอคเกอร์](https://www.docker.com/) อิมเมจ (Docker Image) เพื่อติดตั้งซอฟต์แวร์ที่ต้องใช้ในแต่ละเครื่องไคลแอนต์ ซึ่งอาจารย์ผู้สอนในแต่ละรายวิชาสามารถร้องขอการใช้งานห้องปฏิบัติการคอมพิวเตอร์ได้จากทุกที่ผ่านเว็บแอปพลิเคชัน อีกทั้งยังช่วยประหยัดทรัพยากรของเครื่องคอมพิวเตอร์ไคลแอนต์ได้อีกด้วย

<br>

# **Getting Start**

<br>

ขั้นตอนเเรกทำการ clone project จาก Repository ด้วยคำสั่ง **git clone** ในทุกๆ เครื่อง ทั้ง Master node และ Worker node

<br>

การ Set up แบ่งเป็น 2 ส่วนคือ 
* Setup Kubernetes Cluster 
* Setup Web Application

> ขั้นตอนการติดตั้งดังกล่าว รองรับเฉพาะบนระบบปฏิบัติการ **linux** เท่านั้น

<br>

---

<br>

## **Setup Kubernetes Cluster**

1. เข้าไปที่ Directory script
2. ใน XMPP Server Node ให้ run script ที่ชื่อว่า "xmppserver-setup.sh" ด้วยคำสั่ง ```./xmppserver-setup.sh```

   > หากไม่สามารถ run script ได้ ให้ใช้คำสั่ง chmod +x xmppserver-setup.sh

3. เข้าไปตั้งค่า XMPP Server ที่ http://\<XMPP-Server-IPaddress>:9090
โดยวิธีการตั้งค่าอยู่ในนี้ [Config Openfire](https://edgevpn.io/openfiredocker/) และทำการสร้าง User สำหรับ Master Node และ Worker Node ตามจำนวนที่ต้องการและ สร้าง Group ซึ่งตั้งค่า **Contact List** เป็น **All Users** โดยใน Group มีสมาชิกเพียง 1 Node คือ Master Node

4. ในทุกๆ Node (ทั้ง Master และ Worker) ลง Software EdgeVPN.io โดย run Script ที่ชื่อว่า "setupedgevpn.io.sh" ด้วยคำสั่ง ```./setupedgevpn.io.sh``` โดยใช้ parameter 4 ตัวคือ 

    > หากไม่สามารถ run script ได้ ให้ใช้คำสั่ง chmod +x setupedgevpn.io.sh

    * Host Address ของ XMPP Server 
    * E-mail ของ User ใน XMPP Server
    * Password ของ User ใน XMPP Server
    * Virtual IP Address ที่ต้องการใช้ติดต่อกัน (เช่น 10.10.10.01)
   
    > ตัวอย่างเช่น ./setupedgevpn.io.sh 59.123.5.67 master@gmail.com master 10.10.10.10
    
5. ใน Master Node run Script ที่ชื่อว่า "shellformasternode.sh" ด้วยคำสั่ง ```./shellformasternode.sh``` และเข้าไปแก้ไขไฟล์

    > หากไม่สามารถ run script ได้ ให้ใช้คำสั่ง chmod +x shellformasternode.sh

    ```bash
    /etc/systemd/system/kubelet.service.d/10-kubeadm.conf 
    ```
    โดยทำการเพิ่ม --node-ip ใน KUBELET_CONFIG_ARGS โดยใช้ **Private IP Address** ของเครื่องที่ใช้รัน Script ในขั้นตอนที่ 3
    ```bash
    Environment="KUBELET_CONFIG_ARGS=--config=/var/lib/kubelet/config.yaml --node-ip=<Virtual-IP-Address>"
    ```
    และรันคำสั่ง
    ```bash
    sudo systemctl daemon-reload
    sudo systemctl restart kubelet
    sudo kubeadm init --pod-network-cidr=10.244.0.0/16 --apiserver-advertise-address=<Virtual IP Address ของ Master Node>
    ```
    จากนั้นจะได้คำสั่งและ Token สำหรับการ Join Cluster มาเพื่อเอาไปให้ Worker Node ใช้ในการ Join Cluster
    ตัวอย่างเช่น
    ```bash
    sudo kubeadm join 10.10.10.10:6443 --token yxigqc.vwmi19vbiedklgp7 --discovery-token-ca-cert-hash sha256:590b6698140222b480549e0c7f949ecb4db96c961f388a6377765efe8fde35f1
    ```
    แล้วรันคำสั่ง
    ```bash
    mkdir -p $HOME/.kube
    sudo cp -i /etc/kubernetes/admin.conf $HOME/.kube/config
    sudo chown $(id -u):$(id -g) $HOME/.kube/config
    ```
    ต่อมาคือการ Deploy Pod Network บน Cluster เพื่อทำให้เครื่องภายใน Cluster สามารถติดต่อกันได้โดยใช้คำสั่ง
    ```bash
    sudo kubectl apply -f https://raw.githubusercontent.com/coreos/flannel/master/Documentation/kube-flannel.yml
    ```

6. ในส่วนของ Worker Node run Script ที่ชื่อว่า "setupk8s.sh" ด้วยคำสั่ง ```./setupk8s.sh``` โดยใช้ parameter 1 ตัวคือ 
   
    > หากไม่สามารถ run script ได้ ให้ใช้คำสั่ง chmod +x setupk8s.sh
   
    * HostName ที่ต้องการตั้งให้ Worker Node 

    > ตัวอย่างเช่น ./setupk8s.sh worker11

    และเข้าไปแก้ไขไฟล์

    ```bash
    /etc/systemd/system/kubelet.service.d/10-kubeadm.conf 
    ``` 
    โดยทำการเพิ่ม --node-ip ใน KUBELET_CONFIG_ARGS โดยใช้ **Private IP Address** ของเครื่องที่ใช้รัน Script ในขั้นตอนที่ 3
    ```bash
    Environment="KUBELET_CONFIG_ARGS=--config=/var/lib/kubelet/config.yaml --node-ip=<Virtual-IP-Address>"
    ```
    และใช้คำสั่งที่ใช้ในการ join cluster ที่ได้มาจาก Master Node ในขั้นตอนที่ 4 ตัวอย่างเช่น

    ```bash   
    sudo kubeadm join 10.10.10.10:6443 --token yxigqc.vwmi19vbiedklgp7 --discovery-token-ca-cert-hash sha256:590b6698140222b480549e0c7f949ecb4db96c961f388a6377765efe8fde35f1
    ```
   
   <br>
   
**(Optional)**
   
สามารถเช็คสถานะของเครื่องภายใน Cluster โดยใน Master Node ใช้คำสั่ง 
```bash
kubectl get node
```     
สามารถสร้าง Token ที่ใช้ในการ Join Cluster ใหม่ได้โดยใน Master Node ใช้คำสั่ง 
```bash
kubeadm token create --print-join-command
``` 

<br>

---

<br>

## **Setup Web Application**

### ติดตั้ง Software ที่จำเป็น
* [node version 14.15.4](https://computingforgeeks.com/install-node-js-14-on-ubuntu-debian-linux/)
* [XAMPP](https://www.9lessons.info/2015/12/amazon-ec2-setup-with-ubuntu-and-xampp.html)

### **ขั้นตอนการ Setup Web Application**
1.  สร้าง Database ใน http://\<Master-Node-iP>/phpmyadmin
    ที่มีชื่อว่า **computer_lab** และ Collation เป็น **utf8_general_ci**
    และทำการสร้าง User ใน table users เพื่อใช้ในการเข้าไปจัดการข้อมูลด้านใน
    ```
    username = admin
    email    = admin@vl.edu
    password = $2a$08$cSYX.1gx6KbKUjy8KAdYeOyXENEE0v05HNZdddM5GXN0khZj.WNOm
    name     = admin
    role     = admin
    ```
    โดย password ได้มาจากการ encode text "12345678"
    
    <br>
    
2.  เข้าไปใช้คำสั่ง ```npm install``` ในโฟลเดอร์ Client และ Server และเข้าไปแก้ไขข้อมูลใน
    ```client/src/Config/IpConfig.js``` 
    โดยเปลี่ยนแปลง **SERVER_IP** ให้เป็น IP ของ Master Node
    ``` 
    module.exports = {
          SERVER_IP: "<Master-Node-IP>"
    };
    ``` 
    และสร้าง Directory ที่ชื่อว่า **yaml** และ **servicelist** ในโฟลเดอร์ server

3. ใช้คำสั่ง ```npm start``` ในฝั่ง Client และ Server
4. เข้าไปที่ http://\<Master-Node-IP>:3000
