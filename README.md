import requests
from bs4 import BeautifulSoup
import urllib.request
from pdf2image import convert_from_path
import os


crawling_folder = 'D:\\Users\\user\\Desktop\\crawling'          # crawling 폴더 경로

try:
    os.makedirs(crawling_folder)
except FileExistsError:
    pass


crawling_folder1 = 'D:\\Users\\user\\Desktop\\crawling\\학식_pdf'  # pdf 파일 경로

try:
    os.makedirs(crawling_folder1)
except FileExistsError:
    pass


crawling_folder2 = 'D:\\Users\\user\\Desktop\\crawling\\학식_png'  # png 파일 경로

try:
    os.makedirs(crawling_folder2)
except FileExistsError:
    pass

url = 'https://www.catholic.ac.kr/www/campuslife41_1.html'
response = requests.get(url)
html = response.content.decode('utf-8','replace')

soup = BeautifulSoup(html, 'html.parser')
lis = []
base_url = 'https://www.catholic.ac.kr'
links = soup.find_all('a', class_='ibtn1 block')
yo = soup.find('a', class_='ibtn1 block').text.strip()

lis_pos = ['부온 프란조', '카페 보나', '카페 멘사']

for link in links:
    href = link.get('href').strip()
    full_url = base_url + href
    lis.append(full_url)

for idx, a in enumerate(lis):
    response1 = urllib.request.urlopen(a)
    filename = f"{lis_pos[idx]}_{yo}"
    file = open(f"D:\\Users\\user\\Desktop\\crawling\\학식_pdf\\{filename}.pdf", 'wb')
    file.write(response1.read())
    file.close()

    pdf_file = f"D:\\Users\\user\\Desktop\\crawling\\학식_pdf\\{filename}.pdf"
    images = convert_from_path(pdf_file, dpi=300)
    for i in images:
        png_file = f"D:\\Users\\user\\Desktop\\crawling\\학식_png\\{filename}.png"
        i.save(png_file, 'PNG')













