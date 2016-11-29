# EasyCommonDAO manual

EasyCommonDAO is a Java ORM Common `DAO(Data Access Object)` implementation, can eliminate the interface and implements of DAO.

## Latest Version

- Least version:  `EasyCommonDAO-1.4.0-RELEASE`

- Test enviroment:

 ```
 Hibernate 5.2.4(JPA 2.1)
 Hibernate 4.3.11(JPA 2.1)
 Hibernate 3.6.10(JPA 2.0)
 EclipseLink 2.6.4(JPA 2.0+)
 Spring 4.3.3
 ```

## Featuter

1. Provides common DAO implementation based on **Hibernate Native APIs** and **Java Persistence API (JPA)** (`Hiberante`, `EclipseLink`)

2. Eliminate the definition and implementation of DAO layer interface

3. Two implementation options: **pure ORM DAO implementation**, **Spring ORM DAO seamless integration implementation**

4. Easy to use, low learning costs, Hibernate and JPA use a unified interface specification

5. Provides paging objects (`PageBean`), conditional objects (`EasyCriteria`)

6. Provide cache query; cache management; batch data processing; cascade delete support ...




## EasyCommonDAO Architecture


![EasyCommonDAO](images/Architecture.png)




## Technical comparison

Compared to `Basic DAO` and `Spring Data JPA`, `EasyCommonDAO` does not need to define a DAO interface, nor does it need to learn method naming conventions. Reduce the cost of learning, and truly eliminate the DAO layer to achieve.


- **EasyCommonDAO**

 ![EasyCommonDAO](images/EasyCommonDAO.png)

- **Spring Data JPA**

 ![Spring Data JPA](images/SpringDataJPA.png)

- **Basic DAO**

 ![Basic DAO](images/BasicDAO.png)





## Maven

```XML
<dependency>
    <groupId>cn.easyproject</groupId>
    <artifactId>${EasyCommonDAO.artifactId}</artifactId>
    <version>1.4.0-RELEASE</version>
</dependency> 
```

**Choice one EasyCommonDAO.artifactId**

- Hibernate Native API

 ```
 `easycommondao-hibernate5`
 
 `easycommondao-hibernate4`
 
 `easycommondao-hibernate3`
 ```

- JPA

 ```
 `easycommondao-jpa-hibernate5`
 
 `easycommondao-jpa-hibernate4`
 
 `easycommondao-jpa-hibernate3`
 
 `easycommondao-jpa-eclipselink`
 ```



## Pure ORM use

1. Configure your ORM framework

 - Configure Hibernate3/4/5: `hibernate.cfg.xml`
 
 - Configure JPA(`Hibernate3/4/5`, `EclipseLink`): `persistence.xml`, set `name="EasyCommonDAO"`
 
    ```XML
    <persistence-unit name="EasyCommonDAO" transaction-type="RESOURCE_LOCAL">
    ```

2. Write Service layer code, instance CommonDAO object, invoke CommonDAO API

 ```JAVA
 public class UserServiceImpl implements UserService {
 
      // EasyCommonDAO instance
      public CommonDAO dao=new CommonDAOImpl();
      
      public void get(int userId){
          // Invoke ComonDAO API
          return dao.find(User.class, userId);
      }
 
      // ...
 }
 ```


## Spring integration with ORM

1. Configure Spring DAO, Generally including:
 
 - `DataSource`
 
 - `EntityManagerFactory`/`SessionFactory`
 
 - `TransactionManager` 

3. Configre Spring scann package `cn.easyproject` 

 ```XML
 <context:component-scan base-package="cn.easyproject"></context:component-scan>
 ```

2. Inject CommonDAO component to Service layer `@Resource(name="commonDAO")`, invoke CommonDAO API

 ```JAVA
 public class UserServiceImpl implements UserService {
 
      // Inject EasyCommonDAO instance
      @Resource(name="commonDAO")
      public CommonDAO dao;
      
      public void get(int userId){
          // Invoke ComonDAO API
          return dao.find(User.class, userId);
      }
 
      // ...
 }
 ```







## PageBean pagination
 
If you use **EclipseLink**, the select statement must be specified. 

- Example
 
 ```JAVA
 PageBean pb = new PageBean();

 // FROM Clause; optional
 // **If use EclipseLink is required!**
 pb.setSelect(" select new com.company.ssh.entity.Account(ac.accountid, ac.qxname) ");
 // FROM Clause Entity Name; rquired
 pb.setEntityName("Account ac"); 
 // Page Number; optional; default is 1
 pb.setPageNo(1); 
 // Rows per page; optional; default is 10
 pb.setRowsPerPage(4);
 // WHERE Clause; optional; default is ''
 pb.setCondition(" and ac.accountid>2");
 // Append where clause condition; optional; default is ''
 // pb.addCondition(" and name='A'");
 // SortName; optional; default is ''
 pb.setSort("ac.accountid");
 // SortOrder; optional; default is 'asc'
 pb.setSortOrder("desc");
 // Other sorting methods; optional; default is ''
 pb.setLastSort(",time desc");

 // Execute pagination quries
 commonDAO.findByPage(pb); 
 
 // Pagination data
 System.out.println(pb.getData());
 System.out.println(pb.getPageNo());
 System.out.println(pb.getRowsPerPage());
 System.out.println(pb.getRowsCount());
 System.out.println(pb.getPageTotal());
 ```

- Example2

 ```JAVA
 // Immediate use this query
 // data sql
 pb.setQuery("select ac from Account ac where ac.accountid>=10 and ac.accountid<1000");
// total sql
 pb.setCountSQL("select count(1) from Account ac where ac.accountid>=10 and ac.accountid<1000"); 
  // Page Number; optional; default is 1
 pb.setPageNo(1); 
 // Rows per page; optional; default is 10
 pb.setRowsPerPage(4);

 // Execute pagination quries
 commonDAO.findByPage(pb); 

 // Pagination data
 System.out.println(pb.getData());
 System.out.println(pb.getPageNo());
 System.out.println(pb.getRowsPerPage());
 System.out.println(pb.getRowsCount());
 System.out.println(pb.getPageTotal());
 ```


## EasyCriteria query

1. New your EasyCriteria class, must **extends EasyCriteria implements Serializable**

2. Write your condition by **getCondition()**

 ```JAVA
 public class SysUserCriteria extends EasyCriteria implements java.io.Serializable {
 	// 1. Criteria field
 	private String name;
 	private String realName;
 	private Integer status; // 0 is ON; 1 is OFF; 2 is REMOVED
 
 	// 2. Constructor
 	public SysUserCriteria() {
 	}
 
 	public SysUserCriteria(String name, String realName, Integer status) {
 		super();
 		this.name = name;
 		this.realName = realName;
 		this.status = status;
 	}
 
 	// 3. Condition genertator abstract method implements
 	public String getCondition() {
 		values.clear(); // **Must clear old values**

 		StringBuffer condition = new StringBuffer();
 		if (StringUtils.isNotNullAndEmpty(this.getName())) {
 			condition.append(" and name like ?");
 			values.add("%" + this.getName() + "%");
 		}
 		if (StringUtils.isNotNullAndEmpty(this.getRealName())) {
 			condition.append(" and realName like ?");
 			values.add("%" + this.getRealName() + "%");
 		}
 		if (StringUtils.isNotNullAndEmpty(this.getStatus())) {
 			condition.append(" and status=?");
 			values.add(this.getStatus());
 		}
 		return condition.toString();
 	}
 
 	// 4. Setters&amp;Getters...
 
 }
 ```

3. Find by EasyCriteria

 ```JAVA
 PageBean pageBean = new PageBean();
 pageBean.setEntityName("SysUser users");
 pageBean.setSelect("select users");

 // EasyCriteria
 SysUserCriteria usersCriteria =new SysUserCriteria();
 usersCriteria.setName("A");
 usersCriteria.setStatus(0);
 
 // Find by EasyCriteria
 commonDAO.findByPage(pageBean, usersCriteria);
 ```


## CommonDAO API 

```JAVA

/**
 * SQL Batch
 * @param sql SQL
 * @param values values
 */
public void batchUpdateSQL(String sql, Object[] values);
/**
 * SQL Batch
 * @param sql SQL
 * @param values values
 */
public void batchUpdateSQL(String sql, Object[][] values);

/**
 * Remove persists object
 * 
 * @param cls Class
 * @param id OID
 */
public void remove(Class cls, Serializable id);

/**
 *Remove persists object
 * 
 * @param o Persists object
 */
public void remove(Object o);
/**
 * Batch delete by query statement(not support cascade)
 * @param cls Class
 * @param fieldName fieldName
 * @param values values
 * @return affected rows count
 */
 public Integer deleteByValues(Class cls, String fieldName, Object[] values);
 
 /**
  * Batch delete by query statement(support cascade)
  * @param cls Class
  * @param Field Name
  * @param Values
  */
 public void deleteCascadeByValues(Class cls, String fieldName, Object[] values);
 
/**
 * Clear second cache
 * @param cls Class
 */
public void evict(Class cls);

/**
 * Clear second cache
 * @param cls Class
 * @param id OID
 */
public void evict(Class cls,Serializable id);
/**
 * Clear second cache
 */
public void evictAll();

/**
 * Clear second cache
 * @param collectionRegion Region
 */
public void evictCollectionRegion(String collectionRegion);

/**
 * Clear second cache
 * @param collectionRegion Region
 * @param id OID
 */
public void evictCollectionRegion(String collectionRegion,Serializable id);
/**
 * Clear query cache
 */
public void evictQueries();
/**
 * Clear query cache
 * @param queryCacheRegion Region
 */
public void evictQueries(String queryCacheRegion);


/**
 * Find
 * @param cls Class
 * @param id OID
 * @return Object
 */
public <T> T find(Class cls, Serializable id);

/**
 * Use jpql/hql query
 * @param jpql jpql/hql
 * @param easyCriteria EasyCriteria
 * @return List
 */
public List find(String jpql,EasyCriteria easyCriteria);

/**
 * Use jpql/hql query
 * @param jpql Use jpql/hql query
 * @param values parma values
 * @return List
 */
public List find(String jpql, Object... values);

/**
 * Find all
 * @param cls Classls
 * @return List
 */
public List findAll(Class cls);

/**
 * Find by query cache
 * @param jpql jpql/hql
 * @param easyCriteria EasyCriteria 
 * @return List
 */
public List findByCache(String jpql, EasyCriteria easyCriteria);
/**
 * Find by query cache
 * @param jpql jpql/hql
 * @param values parma values
 * @return List
 */
public List findByCache(String jpql, Object... values);

/**
 * Find by query cache
 * @param jpql jpql/hql
 * @param queryCacheRegion Region
 * @param easyCriteria EasyCriteria 
 * @return List
 */
public List findByCache(String jpql, String queryCacheRegion, EasyCriteria easyCriteria);
/**
 * Find by query cache
 * @param jpql jpql/hql
 * @param queryCacheRegion Region
 * @param values parma values
 * @return List
 */
public List findByCache(String jpql, String queryCacheRegion, Object... values);

/**
 * Pagination query
 * @param pageBean pageBean
 */
public void findByPage(PageBean pageBean);
/**
 * Pagination query
 * @param pageBean pageBean
 * @param easyCriteria EasyCriteria
 */
public void findByPage(PageBean pageBean,EasyCriteria easyCriteria);

/**
 * Pagination query
 * @param pageBean pageBean
 * @param values parma values
 */
public void findByPage(PageBean pageBean,List values);  
/**
 * Find by propertyName
 * @param cls Classls
 * @param propertyName propertyName
 * @param value value
 * @return List
 */
public List findByProperty(Class cls, String propertyName, Object value);  

/**
 * Find by property ignore case
 * @param cls Classls
 * @param propertyName Property name
 * @param value value
 * @return List
 */
public List findByPropertyIgnoreCase(Class cls, String propertyName, String value);

/**
 * Query by Native SQL, return define result class
 * @param sql SQL
 * @param resultClassResult Result class
 * @param easyCriteria EasyCriteria 
 * @return List<resultClass>
 */
public List findBySQL(String sql, Class resultClass, EasyCriteria easyCriteria);

/**
 * Query by Native SQL, return define result class
 * @param sql SQL
 * @param resultClassResult class
 * @param values parma values
 * @return List<resultClass>
 */
public List findBySQL(String sql, Class resultClass, Object... values);

/**
 * Query by Native SQL, return Object[]
 * @param sql SQL
 * @param easyCriteria EasyCriteria 
 * @return List<Object[]>
 */
public List findBySQL(String sql,EasyCriteria easyCriteria);


/**
 * Query by Native SQL, return Object[]
 * @param sql SQL
 * @param values parma values
 * @return List<Object[]>
 */
public List findBySQL(String sql,Object... values);



/**
 * Find count by jqpl/hql
 * @param jpql jpql/hql
 * @param easyCriteria EasyCriteria 
 * @return count
 */
public int findCount(String jpql, EasyCriteria easyCriteria);  

/**
 * Find count by jqpl/hql
 * @param jpql jpql/hql
 * @param values parma values
 * @return count
 */
public int findCount(String jpql, Object... values);  

/**
 * Query by Native SQL, return Map result
 * @param sql SQL
 * @param easyCriteria EasyCriteria 
 * @return List<Map>
 */
public List findMapResultBySQL(String sql,EasyCriteria easyCriteria);  
/**
 * Query by Native SQL, return Map result
 * @param sql SQL
 * @param values parma values
 * @return List<Map>
 */
public List findMapResultBySQL(String sql, Object... values);  
/**
 * Find max page by jpql/hql
 * @param jpql jpql/hql
 * @param rowsPerPage Rows per page
 * @param easyCriteria EasyCriteria 
 * @return Max page
 */
public int findMaxPage(String jpql, int rowsPerPage, EasyCriteria easyCriteria);  
 
/**
 * Find max page by jpql/hql
 * @param jpql jpql/hql
 * @param rowsPerPage Rows per page
 * @param values parma values
 * @return Max page
 */
public int findMaxPage(String jpql, int rowsPerPage, Object... values);  

/**
 * Query by Named Query
 * @param name queryName
 * @param values parma values
 * @return List
 */
public List findNamedQuery(String name, Object... values) ; 
/**
 * Query by Named Query
 * @param name queryName
 * @param queryCacheRegion Region
 * @param values parma values
 * @return List
 */
public List findNamedQueryByCache(String name, String queryCacheRegion, Object... values) ;

/**
 * Find top N
 * @param jpql jpql/hql
 * @param topCount count
 * @param easyCriteria EasyCriteria 
 * @return List
 */
public List findTop(String jpql,int topCount,  EasyCriteria easyCriteria);



/**
 * Find top N
 * @param jpql jpql/hql
 * @param topCount count
 * @param values parma values
 * @return List
 */
public List findTop(String jpql,int topCount, Object... values); 
/**
 * Find scalar value
 * @param jpql jpql/hql
 * @param easyCriteria EasyCriteria 
 * @return Object
 */
public <T> T findVal(String jpql,  EasyCriteria easyCriteria);

/**
 * Find scalar value
 * @param jpql jpql/hql
 * @param values values
 * @return Object
 */
public <T> T findVal(String jpql, Object... values);

/**
 * Get current EntityManager
 * @return EntityManager
 */
public EntityManager getCurrentEntityManager();

/**
 * Get current EntityManagerFactory
 * @return EntityManagerFactory
 */
public EntityManagerFactory getEntityManagerFactory();

/**
 * Initialize Object
 * @param proxy Object
 */
public void initialize(Object proxy) ;

/**
 * Initialize Object form collection
 * @param collection Collection
 */
public void initializeDeep(Collection collection);
/**
 * Update
 * @param o Entity
 */
public void merge(Object o);
/**
 * Save
 * @param o Entity
 */
public void persist(Object o); 
/**
 * Batch update by jqpl/hql
 * @param jpql jpql/hql
 * @param values parma values
 */
public void updateByJpql(String jpql,Object... values);
/**
 * Execute update/delete/insert by JDBC SQL
 * @param sql SQL
 * @param values parma values
 */
public void updateBySQL(String sql,Object...values);
```





## End


[English Readme](doc/readme_en.md)

[The official home page](http://www.easyproject.cn/easycommondao/en/index.jsp 'The official home page')

[Comments](http://www.easyproject.cn/easycommondao/en/index.jsp#donation 'Comments')

If you have more comments, suggestions or ideas, please contact me.

Contact, Feedback, Custom, Train Email：<inthinkcolor@gmail.com>

[http://www.easyproject.cn](http://www.easyproject.cn "EasyProject Home")



We believe that the contribution of each bit by bit, will be driven to produce more and better free and open source products a big step.

**Thank you donation to support the server running and encourage more community members.**

[![PayPal](http://www.easyproject.cn/images/paypaldonation5.jpg)](https://www.paypal.me/easyproject/10 "Make payments with PayPal - it's fast, free and secure!")
