# 프로그래머스 - 가사 검색 
# 문제
친구들로부터 천재 프로그래머로 불리는 **“프로도”**는 음악을 하는 친구로부터 자신이 좋아하는 노래 가사에 사용된 단어들 중에 특정 키워드가 몇 개 포함되어 있는지 궁금하니 프로그램으로 개발해 달라는 제안을 받았습니다.
그 제안 사항 중, 키워드는 와일드카드 문자중 하나인 ‘?’가 포함된 패턴 형태의 문자열을 뜻합니다. 와일드카드 문자인 ‘?’는 글자 하나를 의미하며, 어떤 문자에도 매치된다고 가정합니다. 예를 들어 “fro??”는 “frodo”, “front”, “frost” 등에 매치되지만 “frame”, “frozen”에는 매치되지 않습니다.
가사에 사용된 모든 단어들이 담긴 배열 words와 찾고자 하는 키워드가 담긴 배열 queries가 주어질 때, 각 키워드 별로 매치된 단어가 몇 개인지 **순서대로** 배열에 담아 반환하도록 solution 함수를 완성해 주세요.
## 가사 단어 제한사항
* words의 길이(가사 단어의 개수)는 2 이상 100,000 이하입니다.
* 각 가사 단어의 길이는 1 이상 10,000 이하로 빈 문자열인 경우는 없습니다.
* 전체 가사 단어 길이의 합은 2 이상 1,000,000 이하입니다.
* 가사에 동일 단어가 여러 번 나올 경우 중복을 제거하고 words에는 하나로만 제공됩니다.
* 각 가사 단어는 오직 알파벳 소문자로만 구성되어 있으며, 특수문자나 숫자는 포함하지 않는 것으로 가정합니다.
## 검색 키워드 제한사항
* queries의 길이(검색 키워드 개수)는 2 이상 100,000 이하입니다.
* 각 검색 키워드의 길이는 1 이상 10,000 이하로 빈 문자열인 경우는 없습니다.
* 전체 검색 키워드 길이의 합은 2 이상 1,000,000 이하입니다.
* 검색 키워드는 중복될 수도 있습니다.
* 각 검색 키워드는 오직 알파벳 소문자와 와일드카드 문자인 ‘?’ 로만 구성되어 있으며, 특수문자나 숫자는 포함하지 않는 것으로 가정합니다.
* 검색 키워드는 와일드카드 문자인 ‘?’가 하나 이상 포함돼 있으며, ‘?’는 각 검색 키워드의 접두사 아니면 접미사 중 하나로만 주어집니다.
	* 예를 들어 “??odo”, “fro??”, “?????”는 가능한 키워드입니다.
	* 반면에 “frodo”(‘?’가 없음), “fr?do”(‘?’가 중간에 있음), “?ro??”(‘?’가 양쪽에 있음)는 불가능한 키워드입니다.


## 나의 풀이
결론적으로 이 문제는 효율성에서 틀렸다. 레벨4에 있는 문제 치고 쉬운데? 싶어서 효율성을 의심하긴 했으나, queries와 words를 각각 오름차순으로 정렬하고 하나씩 순회하다가 query의 길이보다 word의 길이가 커지면 종료하는 방식으로 풀면 되겠다 싶었는데, 효율성 1, 2, 3번에서 틀렸다.

**틀린 풀이** 효율성 통과 못함

'''python
def solution(words, queries):
    result = {}
    
    sortedQueries = sorted(queries, key = len)
    words = sorted(words, key = len)
        
    for query in sortedQueries:
        count = 0
        qCount = query.count('?')
        for word in words:
            if qCount == len(query) and len(query) == len(words):
                count += 1
                continue
            if len(query) < len(word):
                break
            elif len(query) == len(word):
                if query[0] == '?': # 접두사
                    if word[qCount:len(word)] == query[qCount:len(query)]:
                        count += 1
                else: # 접미사
                    if word[0:len(word)-qCount] == query[0:len(query)-qCount]:
                        count += 1
        result[query] = count
                    
    answer = []
    for query in queries:
        answer.append(result[query])    
    return answer
‘’’

이 문제는 이분탐색을 이용하여 해결하면 매우 효율적인 코드가 나온다.
일단 words에 담긴 원소의 최대 길이는 10000이므로, 10001개의 인덱스를 가진 array를 초기화한다. 각각의 인덱스에는 해당 인덱스만큼의 길이를 가지고 있는 문자열을 집어넣는다. 예를 들어, array[5]에는 frodo, front, frost, frame, kakao가 들어가게 되고, array[6]에는 frozen이 들어가게 되는 것이다. 그런 다음 array를 정렬하고, queries 배열을 돌면서 queries의 원소와 동일한 길이 단어를 가지고 있는 인덱스에 접근한 후 이분탐색을 한다. 여기서 bisect 이라는 라이브러리를 사용하는데, bisect_left는 정렬된 순서를 유지하면서 리스트 a에 데이터 x를 삽입할 가장 왼쪽 인덱스를 찾는 것이고, bisect_right는 정렬된 순서를 유지하면서 리스트 a에 데이터 x를 삽입할 가장 오른쪽 인덱스를 찾는 데에 사용된다. 같은 길이의 단어들을 담고있는 인덱스가 정렬되어있는 상태임을 이용하여,  쿼리가 들어갈 수 있는 마지막 위치와 첫번째 위치를 구하여 답을 도출할 수 있다. ‘A’와 ‘Z’로 각각 변경하는 이유는 ‘첫번째 위치’ 와 ‘마지막 위치’를 구하기 위함이다.
그런데 이러한 방법만으로는 와일드카드가 접미사에 붙어있을 때를 해결할 수 없다. 따라서 reversed_array, 즉 원소들을 거꾸로 뒤집어 정렬한 배열을 하나 더 생성하고, queries 배열에 담겨있는 원소들도 마찬가지로 뒤집어 판별한다.

'''python
from bisect import bisect_left, bisect_right

def count_by_range(a, left_value, right_value):
    right_index = bisect_right(a, right_value) # 'oo'??  문자열 중 마지막 위치
    left_index = bisect_left(a, left_value) # 'oo'?? 문자열 중 처음 위치
    return right_index - left_index

array = [[] for _ in range(10001)]
reversed_array = [[] for _ in range(10001)]

def solution(words, queries):
    answer = []
    for word in words:
        array[len(word)].append(word)
        reversed_array[len(word)].append(word[::-1])
    
    for i in range(10001):
        array[i].sort()
        reversed_array[i].sort()
        
    for q in queries: 
        if q[0] != '?':
            res = count_by_range(array[len(q)], q.replace('?', 'a'), q.replace('?', 'z'))
        else:
            res = count_by_range(reversed_array[len(q)], q[::-1].replace('?', 'a'), q[::-1].replace('?', 'z'))
        answer.append(res)
    
    return answer
‘’’

