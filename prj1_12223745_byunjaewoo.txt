#!/bin/bash

get_movie_data() {
    movie_id=$1
    grep "^$movie_id|" "$2"
}

get_action_movies() {
    awk -F "|" '$7 == 1 {print $1, $2}' u.item | sort -n | head -n 10
}

get_average_rating() {
    movie_id=$1
    grep "^$movie_id" "$2" | awk -F "\t" '{sum+=$3; count++} END{print sum/count}'
}

delete_imdb_url() {
    awk -F "|" '{print $1 "|" $2 "|" $3 "|" $4 "|" $6 "|" $7 "|" $8 "|" $9 "|" $10 "|" $11 "|" $12 "|" $13 "|" $14 "|" $15 "|" $16 "|" $17 "|" $18 "|" $19 "|" $20 "|" $21 "|" $22}' u.item | head -n 10
}

get_user_data() {
    i=0
    while IFS='|' read -r user_id age gender occupation num; do
        echo "user $user_id is $age years old $gender $occupation"
        ((i++))

        if [ $i -eq 10 ]; then
            break
        fi
    done < u.user
}

modify_release_date() {
    tail -n 10 u.item | awk 'BEGIN{
        FS=OFS="|"
        month["jan"] = "01"
        month["feb"] = "02"
        month["mar"] = "03"
        month["apr"] = "04"
        month["may"] = "05"
        month["jun"] = "06"
        month["jul"] = "07"
        month["aug"] = "08"
        month["sep"] = "09"
        month["oct"] = "10"
        month["nov"] = "11"
        month["dec"] = "12"
    }
    {
        split($3, date, "-")
        month_str = tolower(date[2])
        modified_month = (month_str in month ? month[month_str] : month_str)
        modified_date = date[3] modified_month date[1]
        print $1 "|" $2 "|" modified_date "|"$4"|" $5 "|" $6 "|" $7 "|" $8 "|" $9 "|" $10 "|" $11 "|" $12 "|" $13 "|" $14 "|" $15 "|" $16 "|" $17 "|" $18 "|" $19 "|" $20 "|" $21 "|" $22
    }'
}






get_movies_rated_by_user() {
    echo "Please enter the 'user id' (1~943):"
    read user_id

    movies=$(awk -F '\t' -v id="$user_id" '$1 == id {print $2}' u.data)

    sorted_movies=$(echo "$movies" | tr ' ' '\n' | sort -n | paste -sd "|")

    echo "$sorted_movies"

    echo "$movies" | while IFS=$'\t' read -r movie_id _; do
        movie_title=$(grep -m 1 "^$movie_id" u.item | cut -f 2 -d '|')
        echo "$movie_id|$movie_title"
    done | sort -n | head -n 10
}

get_average_rating_programmers() {
    awk -F '|' '
    BEGIN {
        RS="
    "
        sum = 0
        count = 0
    }
    FILENAME == "u.data" {
        movie_ratings[$2] += $3
        movie_counts[$2]++
    }
    FILENAME == "u.user" {
        if ($2 >= 20 && $2 <= 29 && $4 == "programmer") {
            programmers[$1] = 1
        }
    }
    FILENAME == "u.item" {
        movies[$1] = $2
    }
    END {
        for (movie_id in movie_ratings) {
            if (movie_id in programmers) {
                average = movie_ratings[movie_id] / movie_counts[movie_id]
                printf "%d %.6f\n", movie_id, average
            }
        }
    }' u.data u.user u.item | sort -n
}

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


while true
do
    read -p "Enter your choice [ 1-9 ]: " choice

    case $choice in
        1)
            echo
            read -p "Please enter the 'movie id' (1~1682): " movie_id
            echo
            get_movie_data "$movie_id" "$1"
            ;;
        2)
            echo
            read -p "Do you want to get the data of 'action' genre movies from 'u.item'?(y/n): " answer
            echo
            if [[ $answer == "y" || $answer == "Y" ]]; then
                get_action_movies "$1"
            fi
            ;;
        3)
            echo
            read -p "Please enter the 'movie id' (1~1682): " movie_id
            echo
            average_rating=$(get_average_rating "$movie_id" "$2")
            echo "Average rating of $movie_id: $average_rating"
            ;;
        4)
            echo "Do you want to delete the 'IMDb URL' from 'u.item'?(y/n)"
            read -r choice
            if [[ $choice == "y" || $choice == "Y" ]]; then
                delete_imdb_url
            fi
            ;;
        5)
            echo "Do you want to get the data about users from ‘u.user’?(y/n)"
            read -r choice
            if [[ $choice == "y" || $choice == "Y" ]]; then
                get_user_data
            fi
            ;;
        6)
            echo "Do you want to Modify the format of 'release data' in 'u.item'?(y/n)"
            read -r choice

            if [[ $choice == "y" || $choice == "Y" ]]; then
                modify_release_date
            fi
            ;;
        7)
            get_movies_rated_by_user
            ;;
        8)
            echo "Do you want to get the average 'rating' of movies rated by users with 'age' between 20 and 29 and 'occupation' as 'programmer'?(y/n):"
            read -r choice

            if [[ $choice == "y" || $choice == "Y" ]]; then
                get_average_rating_programmers
            fi
            ;;
        9)
            echo "Bye!"
            exit 0
            ;;
        *)
            echo "Invalid choice. Please try again."
            ;;
    esac

    echo
done
