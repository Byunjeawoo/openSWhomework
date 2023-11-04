# openSWhomework

기본적으로 echo를 이용해
echo "--------------------------"
echo "User Name: fos"
echo "Student Number: 00000000"
echo "[ MENU ]"
echo "1. Get the data of the movie identified by a specific 'movie id' from 'u.item'"
echo "2. Get the data of action genre movies from 'u.item'"
echo "3. Get the average 'rating' of the movie identified by specific 'movie id' from 'u.data'"
echo "4. Delete the 'IMDb URL' from 'u.item'"
echo "5. Get the data about users from 'u.user'"
echo "6. Modify the format of 'release date' in 'u.item'"
echo "7. Get the data of movies rated by a specific 'user id' from 'u.data'"
echo "8. Get the average 'rating' of movies rated by users with 'age' between 20 and 29 and 'occupation' as 'programmer'"
echo "9. Exit"
echo "--------------------------"
기본 양식을 출력.
이후 while문과 case 문으로 choice에 따른 올바른 함수를 호출하는 형식으로 과제를 구현하였습니다. 
Choice 후 y/n에 대한 질문은 if문으로 처리하여 y,Y를 입력한 경우에만 함수가 실행되도록 조건문을 구현하였습니다.
지정된 Choice가 없는 경우 Invalid choice를 출력하며, 다시 입력할 수 있도록 추가 choice 구문을 작성하였습니다.
Choice가 9번일 경우 exit 문을 사용해 작업을 종료하는 프로세스를 구성하였습니다.

각 함수에 대한 설명입니다.
choice 1번 기능) get_movie_data()
특정 '영화 ID'를 사용하여 'u.item' 파일에서 해당 영화의 데이터를 가져옵니다.
주어진 영화 ID와 일치하는 줄을 찾아 반환합니다.

choice 2번 기능)get_action_movies()
'u.item' 파일에서 액션 장르의 영화 데이터를 가져옵니다.
'u.item' 파일에서 각 줄을 '|' 기호로 분리한 후, 7번째 필드가 1인 영화를 찾습니다.
해당 영화의 ID와 제목을 숫자 순서대로 상위 10개까지 반환합니다.

choice 3번 기능)get_average_rating()
특정 '영화 ID'를 사용하여 'u.data' 파일에서 해당 영화의 평균 평점을 계산합니다.
'u.data' 파일에서 행마다 영화 ID, 사용자 ID, 평점 등의 데이터가 '|' 기호로 분리되어 저장되어 있습니다.
주어진 영화 ID와 일치하는 줄을 찾아 해당 영화의 평균 평점을 계산하여 반환합니다.

choice 4번 기능)delete_imdb_url()
'u.item' 파일에서 'IMDb URL'을 삭제합니다.
'u.item' 파일의 각 줄을 '|' 기호로 분리한 후, 필요한 필드만 선택하여 새로운 줄을 생성합니다.
url 필드가 포함되지 않은 채로 출력됩니다.
상위 10개의 영화 데이터를 반환합니다.

choice 5번 기능)get_user_data()
'u.user' 파일에서 사용자 데이터를 가져옵니다.
'u.user' 파일의 각 줄을 '|' 기호로 분리한 후, 사용자 ID, 나이, 성별, 직업 등의 데이터를 추출하여 형식에 맞게 출력합니다.
상위 10명의 사용자 정보를 출력합니다.

choice 6번 기능)modify_release_date()
'u.item' 파일에서 '출시일'의 형식을 변경합니다.
'u.item' 파일의 각 줄을 '|' 기호로 분리한 후, Jan. feb, mar… 등의 문자를 직접 숫자 형식으로 변경해 양식에 맞춰 출력합니다.
가장 최근 10개의 영화 데이터를 반환합니다.

choice 7번 기능)get_movies_rated_by_user()
사용자로부터 '사용자 ID'를 입력받아 'u.data' 파일에서 해당 사용자가 평가한 영화들의 데이터를 가져옵니다.
'u.data' 파일의 각 줄을 '\t' 기호로 분리한 후, 사용자 ID와 일치하는 영화 ID를 추출합니다.
상위 10개의 영화 ID를 정렬하여 반환하고, 각 영화에 대해 'u.item' 파일에서 제목을 찾아 반환합니다.

choice 8번 기능)get_average_rating_programmers()
'u.data', 'u.user', 'u.item' 파일을 사용하여 나이가 20에서 29세이고 직업이 '프로그래머'인 사용자들이 평가한 영화들의 평균 평점을 계산합니다.
'u.data' 파일에서 영화 ID, 평점 데이터를 추출하여 해당 영화의 평점을 누적합니다.
'u.user' 파일에서 나이가 20에서 29세이고 직업이 '프로그래머'인 사용자들을 찾습니다.
'u.item' 파일에서 영화 ID와 제목을 추출하여 저장합니다.
계산된 평균 평점을 영화 ID 순서대로 정렬하여 반환합니다.
원인을 모르겠지만 출력이 안되는 문제를 해결하지 못했습니다.
