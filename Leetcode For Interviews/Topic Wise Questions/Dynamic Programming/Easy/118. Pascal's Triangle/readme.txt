Link : https://leetcode.com/problems/pascals-triangle/description/?envType=problem-list-v2&envId=dynamic-programming

Question : 

Given an integer numRows, return the first numRows of Pascal's triangle.

In Pascal's triangle, each number is the sum of the two numbers directly above it as shown:


 

Example 1:

Input: numRows = 5
Output: [[1],[1,1],[1,2,1],[1,3,3,1],[1,4,6,4,1]]
Example 2:

Input: numRows = 1
Output: [[1]]
 

Constraints:

1 <= numRows <= 30


Soln : Simple soln using just normal , while condition and expanding the answer list by a size of 1 each time , by adding consecutive elements 

Code : 

class Solution {
public:
    vector<vector<int>> generate(int numRows) {
        vector<vector<int>> ans;
        vector<int> go;
        go.push_back(1);
        
        ans.push_back(go);
        if(numRows == 1) {
            return ans;
        }
        go.clear();
        go.push_back(1);
        go.push_back(1);
        ans.push_back(go);
        numRows -= 2;
        
        while(numRows--) {
            go.clear();
            vector<int> prev = ans.back();
            go.push_back(prev[0]);
            for(int i = 0 ; i < prev.size() - 1 ; i ++ ){
                go.push_back(prev[i] + prev[i+1]);
            }
            
            
            
            go.push_back(prev.back());
            for(int i = 0 ; i < go.size() ; i ++ ) {
                cout<<go[i]<<" ";
            }
            
            cout<<"\n";
            ans.push_back(go);
        }
        
        return ans;
    }
};
