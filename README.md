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

3.建立Custmon Gateway (這邊會需填寫GCP的網路介面 ip 目的是讓AWS知道GCP的ip配上Aws通道)
<img width="1375" height="816" alt="image" src="https://github.com/user-attachments/assets/9e0daf22-74cc-4624-bb96-ca79241dd75d" />

<img width="1537" height="700" alt="image" src="https://github.com/user-attachments/assets/986f47de-dd5f-462e-aac6-3c4591da2eaf" />
<img width="1533" height="639" alt="image" src="https://github.com/user-attachments/assets/4e3df798-feaa-470d-8bec-7ec75454415a" />
<img width="1527" height="824" alt="image" src="https://github.com/user-attachments/assets/3f5b7fd6-7d3f-41bf-a658-72a1c0359b6a" />

4. Transi Gateway attachment  再次要使用VPN
   
<img width="1392" height="832" alt="image" src="https://github.com/user-attachments/assets/7087cd9d-d49a-4b1d-bab8-0e0f7edc2bcc" />

<img width="1526" height="826" alt="image" src="https://github.com/user-attachments/assets/b359e14e-ac41-4eb4-9500-faa8e5eff938" />

5. 此時添加完成  attachment VPN 即可查看site to site vpn connection 出現
<img width="1912" height="840" alt="image" src="https://github.com/user-attachments/assets/d16aa8fd-3d59-478f-9e96-dffc06e05c56" />

6.此時回到GCP填寫介面以及建立cloud router
6.1點選vpn connection (site to site vpn connection )進路後勾選下方會有通道詳細資訊 可看到一個連接會有兩個通道ip填寫至GCPVPN 通道
6.2 **須注意** 關於在GCP上填寫時候 一定要先看好 AWS上的vpn connection 對應到的 客戶閘道ip (EX: 客戶閘道ip是 34.55.55.55 下方出現的通道ip是 3.3.3.3 4.4.4.4 在GCP介面上就需要對應好 不然會通不了 )
<img width="1918" height="830" alt="image" src="https://github.com/user-attachments/assets/4af9d1f3-4faa-458b-a5e5-b1300d5a5e7c" />
<img width="1911" height="853" alt="image" src="https://github.com/user-attachments/assets/d77981eb-f301-499c-b4ec-4d2d1a499b26" />
<img width="1913" height="900" alt="image" src="https://github.com/user-attachments/assets/ac643c3b-ceca-4d41-b290-0a044d843546" />
<img width="1816" height="792" alt="image" src="https://github.com/user-attachments/assets/179cbbd0-8e35-4e5b-93b2-98aef172895a" />

6.3接續到AWS CGW(Customer gateway)這邊要拿ikev2 編輯通道選項 選擇對應在GCP上的ip 對應的key確保完全正確 以此類推做四次 因為此次兩個CGW 配上 四個通道
<img width="1911" height="829" alt="image" src="https://github.com/user-attachments/assets/76fa8919-cf28-407b-98ba-b6f5d1dd19a3" />
<img width="1469" height="815" alt="image" src="https://github.com/user-attachments/assets/56ebdefe-871a-4080-ab7a-0c9d58aa9208" />

6.4 接著設定 BGP 工作階段 這邊用通道ip旁邊的私有ip地址 應該會是舉例: 16.1.1.1/30  往後加上兩位數  變成填寫 16.1.1.2 以及 16.1.1.3 主要第一位數是 填寫 16.1.1.2   BGP對等點 ipv4位置    cloud router Bgp ipv4填寫  16.1.1.3
<img width="1903" height="674" alt="image" src="https://github.com/user-attachments/assets/08e4ce6d-bb17-43c0-8fc7-bed93ee14e6b" />
<img width="519" height="921" alt="image" src="https://github.com/user-attachments/assets/ac5ddbe7-465b-4688-b1f9-8753826f154f" />

