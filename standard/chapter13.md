# (十三) 常用写法

**Rule 1. 【强制】List转Map使用java8简洁语法**

```java
 //NOT RECOMMAND
 List<Institution> institutionList=Lists.newArrayList();
            HashMap<Long,Institution> map=Maps.newHashMap();
            for(Institution inst:institutionList){
                map.put(inst.getInstId(),inst);
            }
//RIGHT
 institutionClassList.stream().collect(Collectors.toMap(InstitutionClass::getClassId, (p) -> p, (k, v) -> v));

institutionClassList.stream().collect(Collector.of(HashMap::new, (m, per)->m.put(per.getClassId(),per.getBanner()), (k, v)->v, Collector.Characteristics.IDENTITY_FINISH)); 
k不能为空，v可为空

//toMap
Map<String, Integer> result = people.collect(HashMap::new,(map,p)->map.put(p.name,p.age),Map::putAll);
/*使用Collectors.toMap形式*/
Map<String, Integer> result = people.collect(Collectors.toMap(p -> p.name, p -> p.age, (exsit, newv) -> newv));
```
----

**Rule 2. 【强制】List转List ID使用java8简洁语法**

```java
 List<Long> gmtFilterStudentIds = studentList.stream()
                        .map(InstitutionStudent::getStudentId).distinct().collect(Collectors.toList());
```

**Rule 3. 【强制】List分组使用java8简洁语法groupingBy**

```java
//替代ArraylistMultimap
ArrayListMultimap<Long, InstitutionClassSchedule> classScheduleTimeMap = ArrayListMultimap.create();
for (InstitutionClassSchedule institutionClassScheduleTime : scheduleTimeList) {
    classIdSet.add(institutionClassScheduleTime.getClassId());
    classScheduleTimeMap.put(institutionClassScheduleTime.getClassId(), institutionClassScheduleTime);
}
Map<Long,List<InstitutionClassSchedule>> classScheduleTimeMap2=scheduleTimeList.stream().collect(Collectors.groupingBy(InstitutionClassSchedule::getClassId));

//还可以进行其它比如求和的操作，主要是Collectors里的一些操作
Map<String, Double> avgSalesByCity =
  employees.stream().collect(groupingBy(Employee::getCity,
                               averagingInt(Employee::getNumSales)));
```

**Rule 4. [强制] 使用java8语法减少空指针**
```java
Optional<String> op1 = Optional.of("Hello");
//可以直接处理
Optional<String> op2 = op1.map((s) -> s.toUpperCase());
//非空的情况下逻辑
op2.ifPresent((s) -> {
  System.out.println(s); // 输出 HELLO
});

Optional<String> op1 = Optional.of("Hello");
System.out.println(op1.orElseGet(() -> {return new String("World");})); // 输出 Hello
Optional<String> op2 = Optional.ofNullable(null);
System.out.println(op2.orElseGet(() -> {return new String("World");})); // 输出 World
```