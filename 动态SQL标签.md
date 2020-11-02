动态SQL标签
if
choose(when,otherwise)
trim(where,set)
foreach



Mybatis采用基于OGNL的表达式来淘汰其他大部分元素

@Param("")起个标识名

if：根据不同条件查询所有员工

```
<select id="selectEmpByIf" resultType="com.aaa.sqlMapper.Emp">
      select * from emp where 1=1
         <if test="en!=null and en!=''">
             and ename like '%'||#{en}||'%'
         </if>
<select>         
```





choose:部门编号查询部门信息，有的查出来，没的默认40

```
<select id="selectDept" parameterType="int" resultType="com.aaa.model.Dept">
        select * from dept
        <where>
            <choose>
                <when test="dn==10">deptno=10</when>
                <when test="dn==20">deptno=20</when>
                <when test="dn==30">deptno=30</when>
                <otherwise>dept=40</otherwise>
            </choose>
        </where>
    </select>
```



set:根据员工编号改名称，工作，日期

```
 <update id="updateEmp">
       update  emp
       <set>
           <if test="ena!=null and ena!=''">
               ename=#{ena},
           </if>
           <if test="j!=null and j!=''">
               job=#{j},
           </if>
           <if test="d!=null and d!=''">
               hiredate=#{d},
           </if>
       </set>
      where empno=#{eno}
   </update>
```

trim有四个属性

<trim prefix="前缀" prefixOverrides="前缀覆盖" suffix="后缀"suffixOverrides="后缀覆盖"></trim>

trim——1：if标签+where标签用的案例：根据不同条件查询所有员工

```
<!--Trim 代替where-->
    <select id="selectEmpByTrim" resultType="com.aaa.sqlMapper.Emp">
        <trim prefix="select * from emp where" prefixOverrides="and | or">
            <if test="en!=null and en!=''">
                and ename like '%'||#{en}||'%'
            </if>
            <if test="j!=null and j!=''">
                and job=#{j}
            </if>
            <if test="sd!=null">
                and hiredate>#{sd}
            </if>
            <if test="ed!=null">
                <![CDATA[
         and hiredate<#{ed}
              ]]>
            </if>
        </trim>
    </select>
```

trim——2

```
 <select id="selectEmpByTrim" resultType="com.aaa.sqlMapper.Emp">
        <trim prefix="select * from emp where 1=1">
            <if test="en!=null and en!=''">
                and ename like '%'||#{en}||'%'
            </if>
            <if test="j!=null and j!=''">
                and job=#{j}
            </if>
            <if test="sd!=null">
                and hiredate>#{sd}
            </if>
            <if test="ed!=null">
                <![CDATA[
         and hiredate<#{ed}
              ]]>
            </if>
        </trim>
    </select>
```

