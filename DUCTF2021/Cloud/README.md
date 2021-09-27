# Challenge name: Bad Bucket
### Description:
Aw yea have you guys SEEN my new website... its nearly done I swear! I've uploaded it to the ☁️CLOUD☁️ and shared it with you guys now so you can see it! Check it out here

## Exploit

Because the website is store in cloud so I try to go to https://storage.googleapis.com/the-bad-bucket-ductf/

![image](https://user-images.githubusercontent.com/69805864/134938211-fae73210-df72-466b-961c-54c9168a1bac.png)

Looking through all tag and I found a suspicious tag

![image](https://user-images.githubusercontent.com/69805864/134938595-f7b62cf7-8dfc-4d75-8c28-bc1179163b66.png)

Let's add `buckets/.notaflag` to URL which will be `https://storage.googleapis.com/the-bad-bucket-ductf/buckets/.notaflag`

![image](https://user-images.githubusercontent.com/69805864/134939463-3c6c78c7-58d7-4d27-b3ca-30ff7ab4f425.png)


## Flag
DUCTF{if_you_are_beggining_your_cloud_journey_goodluck!}

# ==========================================================================================

# Challenge name: Not as Bad Bucket
### Description:
Okay fine I admit it, we didn't invest in security in my previous website and we learnt our lesson. Luckily we had a Professional Cloud Architect, architect our new security strategy for our website 2.0!

## Exploit

Click the link provided and we see that we can not access the site

![image](https://user-images.githubusercontent.com/69805864/134940371-8158ac22-2f00-4d73-b160-e2475c0f99e1.png)

So let's go to the Google Cloud buckets by accessing the URL `https://console.cloud.google.com/storage/ductf-not-as-bad-ductf/`

![image](https://user-images.githubusercontent.com/69805864/134941115-b765bfdb-887f-4278-b096-e5deacac9a18.png)

When I cicked `pics/` and foud the flag.txt here.

![image](https://user-images.githubusercontent.com/69805864/134941413-3d26e5e7-1148-4750-9a7b-0c831dc2c5c6.png)

I tried to access it but unsuccess

![image](https://user-images.githubusercontent.com/69805864/134941605-8c502e48-5f00-44fe-97b8-3cbeb655e0d3.png)

I returned to the consolse cloud and try click on download button

![image](https://user-images.githubusercontent.com/69805864/134942000-2c3d1c43-2698-4ffd-95b0-c95907640e17.png)

Ohh, no file is downloaded but it print flag here~~

![image](https://user-images.githubusercontent.com/69805864/134942195-493d1c6d-9fdd-4615-b491-233ac7fd64e5.png)


## Flag
DUCTF{all_AUTHENTICATED_users_means_ALL_AUTHENTICATED_USERS_silly}
