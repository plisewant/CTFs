<img width="5550%" src="">
</br>

<img width="344" height="186" alt="Image" src="https://github.com/user-attachments/assets/16dd5b67-dd63-4cc4-b612-ecee52f7246e" />
<br>
  
# Task 1
<img width="576" height="248" alt="Image" src="https://github.com/user-attachments/assets/c426a71b-7f31-4853-a640-421493c56689" />
<br>
>Hint : Scan the host with nmap.
<br>
How many TCP ports are open? <br>
해당 ip에 열린 포트가 있는지 확인하기 위해 nmap명령어를 사용한다.
<br>

```
nmap -p- -sV 10.10.10.245
```
-p- 옵션은 모든 포트 확인, -sV는 서비스, 버전확인

<br>
<img width="1190" height="383" alt="Image" src="https://github.com/user-attachments/assets/0da9dbcf-94ed-4453-8bd8-fa919fcf89b6" />

<br>

```
nmap -p- -sS -sV --min-rate 5000 10.10.10.245
```
-p-: 모든 포트 스캔<br>
-sS: SYN 스캔<br>
-sV: 서비스, 버전확인<br>
--min-rate 5000: 초당 5000패킷<br>
답: 3<br>
<br><br><br>

# tesk 2
After running a "Security Snapshot", the browser is redirected to a path of the format /[something]/[id], where [id] represents the id number of the scan. What is the [something]?
<br>
<img width="1507" height="307" alt="Image" src="https://github.com/user-attachments/assets/c4d06744-0c13-423d-ad19-dc025a6ac879" />
<br>
