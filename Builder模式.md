Builder模式
===

- **定义**：将一个复杂对象的**构建与他的表示分离**，使得同样的构建过程可以创建不同的表示。
- **使用场景**：
	1. 相同的方法，不同的执行顺序，产生不同的事件结果时
	2. 多个部件或零件，都可以装配到一个对象中，但是产生的运行结果又不相同时
	3. 产品类非常复杂，或者产品类中的调用顺序不同产生了不同的作用，这个时候用建造者模式非常合适
	4. 当初始化一个对象特别**复杂**，如**参数多**，且很多参数都具有默认值时
- 去除Diretor角色的**变种builder模式**：
```java
public class Person{
	//属性不可变
	//必要参数
	private final int id;
	private final String name;
	//可选参数
	private final int age;
	private final String sex;
	private final String phone;
	private final String address;
	private final String desc;

	//构造方法私有，调用者不能直接创建Person对象
	private Person(Builder builder){
		this.id = builder.id;
		this.name = builder.name;
		this.age = builder.age;
		this.sex = builder.sex;
		this.phone = builder.phone;
		this.address = builder.address;
		this.desc = builder.desc;
	}

	//可设置相应的getters方法

	//静态内部类
	public static class Builder{
		//必要参数
		private int id;
		private String name;
		//可选参数
		private int age;
		private String sex;
		private String phone;
		private String address;
		private String desc;

		public Builder(int id, String name){
			this.id = id;
			this.name = name;
		}

		public Builder age(int val){
			this.age = val;
			return this;
		}

		public Builder sex(int val){
			this.sex= val;
			return this;
		}

		public Builder phone(int val){
			this.phone = val;
			return this;
		}

		public Builder address(int val){
			this.address= val;
			return this;
		}

		public Builder desc(int val){
			this.desc= val;
			return this;
		}

		public Person build(){
			return new Person(this);
		}
	}
}

//测试
public class Test{
	public static void main(String[] args){
		//链式调用，可读性更佳
		Person p = new Person.Builder(1,"Yau")
					.age(18)
					.sex("女")
					.desc("测试使用")
					.build();
	}
}
```
- **补充**：由于Builder是非线程安全的，所以如果要在Builder内部类中检查一个参数的合法性，必需要在**对象创建完成之后**再检查。
```java
//正例
public Person build(){
	Person p = new Person(this);
	if(p.getAge()>120){
		throw new IllegalStateException("age out of range");
	}
	return p;
}
//反例
public Person build(){
	if(age()>120){
		throw new IllegalStateException("age out of range");
	}
	return new Person(this);
}
```
- **优点**：
	1. 良好的封装性，使用建造者模式可以使客户端不必知道产品内部组成的细节
	2. 建造者独立，容易扩展
- **缺点**：会产生多余的Builder对象以及Director对象，消耗内存