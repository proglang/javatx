class InstanceObject extends Object{
isInstanceOfString(){
return (t->(f -> f)); //False
}
}

class String extends InstanceObject{
isInstanceOfString(){
return (t->(f -> t)); //True
}

class EmptyList<Y> extends List<Y>{
add(x){

}
}

class Test{
	refinement(ls){
		ls.isInstanceOfString() // t -> f -> t
			.apply((x -> x.toString()))
			.apply((x -> ""))
			.apply(ls);
	}
}

if(ls instanceof String){
	return ls.toString();
}else{
	return "";
}
