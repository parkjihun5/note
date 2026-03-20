## API practice

import requests
from pprint import pprint

URL = "https://api.themoviedb.org/3/movie/now_playing"
API_KEY = " "


params = {
    'language' : 'ko-kr',
    # 'api_key' : "짧은 그 키"
}

headers = {
    "Authorization" : f"Bearer {API_KEY}"
}

try:
    response = requests.get(URL, headers=headers, params=params)

    response.raise_for_status()
    # if not response.status_code == 200:
    #     raise Exception
    
    data = response.json()

    data = data

    pprint(data)
    
except Exception as e:
    print(e)

### 현재 상영 중인 영화(Now Playing) 중 평점(vote_average)이 가장 높은 영화를 찾으시오.

(code)
import requests
from pprint import pprint

URL = "https://api.themoviedb.org/3/movie/now_playing"
API_KEY = " "

params = {
    'language' : 'ko-kr',
    # 'api_key' : "짧은 그 키"
}

headers = {
    "Authorization" : f"Bearer {API_KEY}"
}

try:
    response = requests.get(URL, headers=headers, params=params)

    response.raise_for_status()
    # if not response.status_code == 200:
    #     raise Exception
    
    data = response.json()

    data = data['results']

    # [영화, 영화, 영화]

    max_vote_average = 0
    max_vote_average_movie = None

    for movie in data:
        # pprint(movie)
        title = movie['title']
        vote_average = movie['vote_average']
        # print(title, vote_average)
        if max_vote_average < vote_average:
            max_vote_average = vote_average
            max_vote_average_movie = title
    print(max_vote_average)
    print(max_vote_average_movie)
    
except Exception as e:
    print(e)

### 현재 상영 중인 영화(now_playing) 데이터에서 다음 정보만 담긴 리스트를 만드세요.
- title (영화 제목)  
- vote_average (평점)  
- release_date (개봉일)

(code)
import requests
from pprint import pprint

URL = "https://api.themoviedb.org/3/movie/popular"
API_KEY = " "


params = {
    'language' : 'ko-kr',
}

headers = {
    "Authorization" : f"Bearer {API_KEY}"
}

try:
    response = requests.get(URL, headers=headers, params=params)

    response.raise_for_status()
    
    data = response.json()

    new_lst = []
    data = data['results']
    
    for movie in data:
        new_dict = {"title" : movie['title'], "vote_average" : movie['vote_average'], "release_date" : movie['release_date']}
        new_lst.append(new_dict)
            
    pprint(new_lst)
    
except Exception as e:
    print(e)