## Approval Test


- **Maven** 의 dependency

``` xml
<dependency>
            <groupId>com.approvaltests</groupId>
            <artifactId>approvaltests</artifactId>
            <version>9.5.0</version>
        </dependency>
```


- ``import org.approvaltests.Approvals`` 



``` java
import org.approvaltests.Approvals;
import org.junit.jupiter.api.Test;

import static org.junit.jupiter.api.Assertions.assertEquals;

public class SampleTests
{
    @Test
    public void testNormalJunit()
    {
        assertEquals(5, 5);
    }
    @Test
    public void testWithApprovalTests()
    {
        Approvals.verify("Hello World");
    }
    /**
     *  note: this requires GSON which is currently added in the pom.xml file.
     *  This is only required if you want to use the VerifyAsJson.
     **/
    @Test
    public void testJson()
    {
        Person hero = new Person("jayne", "cobb", true, 38);
        Approvals.verifyAsJson(hero);
    }
}
```