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






****************************************************   V.V imp learning 

While solving this i got to know about an interesting fact 

in hashMap and HashSet we use buckets right , to keep the elements or the keys rather 
so i only overrode the method equals but not the method hashCode which was also suggested to me by chatgpt 

Then i asked chatgPT , THAT what happens if i dont override the hasHcode method when am using a custom class as a Key in hashMap or an elemnt in a set 

HashCode is used to decide which bucket this element or key will go into , 
and whithin that key this equals method will be used , to check if key already exists , 
imagine , if hashCode is generated differently then the keys having same (x , y) will endup in the different bucket , and even though you have written the equals method it wont work 
so , HashCode method overriding is a must 


Golden rule ,- > whenever overriding equals method , also override hashCode method 

@Override
int hashCode() {
  return Objects.hash(x , y);
}


class Position {
        int x ; 
        int y;

        public Position(int x , int y ) {
            this.x = x;
            this.y = y;
        }

        @Override
        public boolean equals(Object o ) {
            if(o == null ) {
                return false;
            }
            if(! (o instanceof Position)) {
                return false;
            }
            Position other = (Position) o;
            return this.x == other.x && this.y == other.y;
        }

        @Override
        public int hashCode() {
            return Objects.hash(x , y);
        }

    }













