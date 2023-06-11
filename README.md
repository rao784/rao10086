# rao10086

题目详情
Scanner s =new Scanner(System.in); 
		String input=s.nextLine();
		String a=input.replace(" ","");
		int output=0;
		boolean add = true;
		String[]  strs=a.split("\\+|\\-");
		String[] str1=a.split("\\d");
		int i=0;
		for(String a1:strs) {
			if(!str1[i].equals("-")) {
				add=true;
			}else {
				add=false;
			}
			i++;
			if(add) {
			output=output+Integer. valueOf(a1);
			}else{
				output=output-Integer.valueOf(a1);
			}
		}
		System.out.println(output);
