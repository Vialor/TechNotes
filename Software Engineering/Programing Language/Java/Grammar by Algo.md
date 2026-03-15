```Java
// Leetcode 127
class Solution {
    public int ladderLength(String beginWord, String endWord, List<String> wordList) {
        Set<String> wordSet = new HashSet<>(wordList);

        HashMap<String, Integer> wordLadderLen = new HashMap<>();
        wordLadderLen.put(beginWord, 1);

        Queue<String> bfsQueue = new ArrayDeque<>();
        bfsQueue.offer(beginWord);

        Function<String, List<String>> findNeighbors = (String word) -> {
            List<String> neighbors = new ArrayList<>();
            for (int i = 0; i < word.length(); i++) {
                for (int j = 0; j < 26; j++) {
                    String neighbor = word.substring(0, i) + (char)('a' + j) + word.substring(i + 1);
                    if (wordSet.contains(neighbor)) {
                        neighbors.add(neighbor);
                    }
                }
            }
            return neighbors;
        };

        while (!bfsQueue.isEmpty()) {
            String cur = bfsQueue.poll();
            Integer curLen = wordLadderLen.get(cur);
            for (String neighbor : findNeighbors.apply(cur)){
                if (neighbor.equals(endWord)) {
                    return curLen + 1;
                }
                if (!wordLadderLen.containsKey(neighbor)) {
                    bfsQueue.offer(neighbor);
                    wordLadderLen.put(neighbor, curLen+1);
                }
            }
        }
        return 0;
    }
}
```