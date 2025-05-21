# Database-Systems-CSCI-4380-01-Homework-7-solution

Download Here: [Database Systems, CSCI 4380-01 Homework # 7 solution](https://jarviscodinghub.com/assignment/database-systems-csci-4380-01-homework-7-solution/)

For Custom/Original Work email jarviscodinghub@gmail.com/whatsapp +1(541)423-7793

In this homework, you will use a new data format that mimics have tables and indices are stored on disk. You are given multiple folders. Each one contains the same data, tuples in the clues table, sorted diﬀerently on disk. There is also pages for a single index in each folder, the index is on clues(gameid, clueid, category).
Data pages. The tuples in the clues table are stored in diﬀerent ﬁles named page#.txt, representing disk pages. Each such ﬁle contains a number of tuples, one each line, ﬁelds are separated by |. The ﬁelds are ordered as follows:
gameid, clueid, clue,value,category, cat type, isdd, correct answer
Index pages. Each index page correspoding to a speciﬁc node is also stored in a separate ﬁle, named index#.txt except for the root that is named index root.txt.
Each index page has a header that lists whether it is it a Leaf or an Internal node. Leaf nodes have a sibling pointer, the name of the ﬁle containing the next leaf node, e.g. Leaf|index6.txt. The last leaf node has no sibling pointer and will have the value Leaf|-.
Each line of an index page contains an entry and a pointer. The entry is the value of the indexed attributes.
Pages containing leaf nodes point to the page containing the indexed tuple:
1110|4|MY NAME IS URL|page222.txt
1
means that a tuple for gameid=1100, clueid=4, category=’MY NAME IS URL’ is stored in data page page222.txt.
Pages containing internal nodes contain pointers to other index pages:
2897|5|IT AIN’T BRAIN SURGERY|index418.txt
means that the leaf node index418.txt points to entries with the smallest value gameid=2897, clueid=5, category=’IT AIN’’T BRAIN SURGERY’.
Remember, index is on clues(gameid, clueid, category), which means the tuples are sorted by gameid ﬁrst, then by clueid next and then by category. Given that gameid and clueid is unique in this relation, sorting by category does not have an eﬀect. It is there as additional information.
Alternate Folders. You are given three separate folders, data1, data2, data3. Each one contains the same information but the underlying table is stored with respect to a diﬀerent order. This will impact the query costs. For each query given to you, you must assess the query costs for each folder separately and report.
Problem Speciﬁcation
Your job in this homework is to use the index to answer a query and then output the cost of this query based on the data in each of the three folders. You do not need to return the answer to the query and you can assume the number of levels of the B-tree is ﬁxed.
The cost of a query is given by the number of disk pages (i.e. ﬁles) that must be read to answer this query.
Each query is answered by scanning the index ﬁrst, starting with root, an internal node and the ﬁrst leaf node. If the range spans multiple leaf nodes, then you will use the sibling pointers to scan towards right until a value outside of the range is found.
For each found tuple, you will read the data pages if necessary (i.e. the query requests an attribute that is not in the index). If the same data page is found for multiple tuples, you can assume you read a data page only once.
For each query, print out number of tuples found, number of index pages (including root) and data pages read.
Query Syntax. Each query speciﬁes a value or range for three attributes gameid, clueid, category and a list of attributes to return. If there is no upper/lower range for a condition, it is listed as empty using Python slicing syntax.
For example: for gameid:
• [10:10]: same as gameid=10. • [:20]: same as gameid<=20. • [30:]: same as 30<=gameid. • [40:50]: same as 40<=gameid and gameid<=50. 2 • [:]: means all gameid values are allowed. So, the following query: [10:10]|[3:4]|[:]|gameid,clueid,clue is equivalent to: select gameid, clueid, clue from clues where gameid=10 and 3<=clueid and clueid<=4 [:]|[:]|[A:B]|category,clue is equivalent to: select category, clue from clues where 'A' <= category and category <= 'B' Submission Instructions. You will submit two ﬁles to SUBMITTY: your program output as a text ﬁle and your program source code. You will be a given an input ﬁle with diﬀerent test conditions before the deadline. Each line of the ﬁle will contain a query of the above form. Run this against your code and submit your output. Please math our format because we will use diﬀ to test its correctness. In your output ﬁle, you will list the query on one line, followed by three lines, one for the query cost for each folder, followed by a single new line. Name this text ﬁle username_hw7ans.txt and submit it. Suppose you have the following input ﬁle: 2140:2140|:|:|category 2140:2140|:|:|clue,category Here is how the output ﬁle will look (correctness is not yet guaranteed, but I will post a few answers later in the week): Query: 2140:2140|:|:|category Folder:hw7_data/data1 Index Pages: 3 Data Pages: 0 Folder:hw7_data/data2 Index Pages: 3 Data Pages: 0 Folder:hw7_data/data3 Index Pages: 3 Data Pages: 0 Query: 2140:2140|:|:|clue,category Folder:hw7_data/data1 Index Pages: 3 Data Pages: 1 Folder:hw7_data/data2 Index Pages: 3 Data Pages: 4 Folder:hw7_data/data3 Index Pages: 3 Data Pages: 4
