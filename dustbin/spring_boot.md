[TOC]

#  单测集成

```java
@RunWith(SpringRunner.class)
@SpringBootTest
public class FooApplicationTests {
    private MockMvc mvc;
  
  	@Before
  	public void setup() {
        this.mvc = MockMvcBuilders.standaloneSetup(new IndexController()).build();
    }
  
  	@Test
  	public void contextLoads() throws Exception {
        RequestBuilder req = MockRequestBuilders.get("/index");
      
      	mvc.perform(req).andExpect(MockRequestBuilders.status().isOk())
          .andExpect(content().string("hello world"));
    }
}

@RestController
@RequestMapping("/index")
public class IndexController {
    @RequestMapping
  	public String hello() {
        return "hello world";
    }
}
```



