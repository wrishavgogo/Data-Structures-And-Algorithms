I was doing a question where i needed to create an object HashMap in java , and while doing so 
i wondered

class Position {
  int x , y;
  public Position(int x , int y ) {
        this.x = x;
        thix.y = y;
    }
}

Map<Position , Integer> mp 

When am creating this to compare with keys , in java we would need some comparison like in c++ we dont but in java we need to write an equals method to mention it in java 


so yes we need it 

there are two methods in the Object class which is implicityly inherited by all the classes 

class Position {
  int x , y;
  public Position(int x , int y ) {
        this.x = x;
        thix.y = y;
    }

  public boolean equals(Object o) {
    if(o == null ) return false;
    if( ! o instanceof Position) return false; -----> instanceof is a single keyword , not instance of ,, i did this mistake
    // directly compare values now 
    Position other = (Position) o --> we need to type case it to make the attribute calls
    return this.x == o.x && this.y == o.y;
  }
}
