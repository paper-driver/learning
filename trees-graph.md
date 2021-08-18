# Trees and Graphs
## Sequence Transformation
A transformation sequence from word beginWord to word endWord using a dictionary wordList is a sequence of words beginWord -> s1 -> s2 -> ... -> sk such that:

* Every adjacent pair of words differs by a single letter.
* Every si for 1 <= i <= k is in wordList. Note that beginWord does not need to be in wordList.
* sk == endWord

Given two words, beginWord and endWord, and a dictionary wordList, return the number of words in the shortest transformation sequence from beginWord to endWord, or 0 if no such sequence exists.

### Solution 1: cause timeout 
```java
class Solution {
    public int ladderLength(String beginWord, String endWord, List<String> wordList) {
        // consider the case we just find one possible sequence from beginWord to endWord
        // get a first word that only has one character difference from beginWord. -> issue another ladderLength
        // make sure the selected word is removed from wordList, other wise it will cause the infinite loop

        // make sure endWord is in wordList

        if(!wordList.contains(endWord)){
            return 0;
        }

        if(checkDifference(beginWord, endWord) == 1){
            // for the scenario that there is one char difference between beginWord and endWord;
            return 2;
        }

        //if there is beginWord, we should remove it since it will not help us to find the sequence.
        wordList.remove(beginWord);

        int ans = 0;
        for(String word : wordList){
            if(word.length() == beginWord.length()){
                int count = checkDifference(beginWord, word);
                if(count == 1){
                    // one char difference means this word can be the candidate
                    List<String> nxtWordList = new ArrayList();
                    int idx = wordList.indexOf(word);
                    nxtWordList.addAll(wordList.subList(0, idx));
                    nxtWordList.addAll(wordList.subList(idx+1, wordList.size()));
                    int ansCandidate = ladderLength(word, endWord, nxtWordList);
                    if(ansCandidate != 0){
                        if(ans == 0 || ansCandidate + 1 < ans){
                            ans = ansCandidate + 1;
                        }
                    }
                }
            }
        }

        return ans;

    }

    public int checkDifference(String beginWord, String endWord){
        // under the assumption that two strings have the same length
        int count = 0;
        for(int i = 0; i < beginWord.length(); i++){
            if(beginWord.charAt(i) != endWord.charAt(i)){
                count ++;
            }
        }
        return count;
    }
}
```

### Solution 2: Breadth First Search
```java
class Solution {
    public int ladderLength(String beginWord, String endWord, List<String> wordList) {

        int L = beginWord.length();

        // a dictionary to hold combinations of words that can be formed from any given word.
        Map<String, List<String>> allComboDict = new HashMap<>();

        for(String word : wordList){
            // all unique word
            for(int i = 0; i < L; i++){
                String newWord = word.substring(0, i) + "*" + word.substring(i + 1, L);
                // all possible next word from selected word
                List<String> lst = allComboDict.getOrDefault(newWord, new ArrayList<>());
                lst.add(word);
                allComboDict.put(newWord, lst);
            }
        }

        // a queue to track how many words being used to constructe a sequence
        Queue<Pair<String, Integer>> q = new LinkedList();
        q.add(new Pair(beginWord, 1));
        
        // avoid adding the same item to the queue we track.
        List<String> visitedWords = new ArrayList();
        visitedWords.add(beginWord);

        while(!q.isEmpty()){
            Pair<String, Integer> node = q.remove();
            String word = node.getKey();
            int level = node.getValue();
            for(int i = 0; i < L; i++){
                String newWord = word.substring(0, i) + "*" + word.substring(i + 1, L);
                for(String adjacentWord : allComboDict.getOrDefault(newWord, new ArrayList<>())){
                    if(adjacentWord.equals(endWord)){
                        // if we found out the curernt word can lead to the endWord, we end
                        return level + 1;
                    }

                    if(!visitedWords.contains(adjacentWord)){
                        visitedWords.add(adjacentWord);
                        q.add(new Pair(adjacentWord, level + 1));
                    }
                }
            }
        }
        return 0;
    }


}
```
{1, }{12, }

2