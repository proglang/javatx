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

class List<Y>{
	<X extends Y> List<X> filter(p){
		List<X> ret;
		for(i in this)if(p(i)){ret.add(i);}else{}
		return ret;
	}
	
	Optional<F> filter
}


class Test{
	refinement(ls){
		ls.isInstanceOfString().apply((x -> x.toString())).apply((x -> "")).apply(ls);
	}
}

if(ls instanceof String){
	return ls.toString();
}else{
	return "";
}
