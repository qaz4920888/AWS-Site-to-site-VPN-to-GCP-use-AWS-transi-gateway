# AWS-Site-to-site-VPN-to-GCP-use-AWS-Transi-Gateway

## 建立AWS /GCP VPC subnet routable igw 

1.<img width="1725" height="647" alt="image" src="https://github.com/user-attachments/assets/de184969-0dc6-4be8-85df-506ecfeaf446" />
<img width="1610" height="830" alt="image" src="https://github.com/user-attachments/assets/2fca63b3-2f25-42e4-8aea-340715f652e6" />





## 建立GCP HA-VPN (為了稍後AWS Custmon-Gateway 要填寫對端GCP 的 網路介面 ip) 
1. <img width="986" height="800" alt="image" src="https://github.com/user-attachments/assets/8be6515a-82ef-4619-87ea-e46ca9fb51a5" />



## 建立Transi-Gateway(此次用TGw 傳統使用VGW )

1.建立Transi Gateway 
<img width="1571" height="825" alt="image" src="https://github.com/user-attachments/assets/8642e6bb-46de-4d97-b90c-16bf8eeb2ac0" />

2. 建立Transi Gateway attachment ( 目的是為了讓 VPC 關連到 Transi Gateway  )
<img width="1526" height="793" alt="image" src="https://github.com/user-attachments/assets/8c450ee0-e82e-4787-aeb4-35bb78acb807" /

3.建立Custmon Gateway (這邊會需填寫GCP的網路介面 ip 目的是讓AWS知道GCP的ip)







