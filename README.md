Sure, I can guide you through creating a comprehensive JUnit testing setup for your Java application using a constants class for shared test data. Below, I will illustrate how to define your constants class and then provide example JUnit tests for typical controller, service, and repository classes in your application.

### Step 1: Define the Constants Class

First, let's create a `TestData` class that contains all the shared constants used across your tests.

```java
public class TestData {
    public static final Long DEALER_ID = 1L;
    public static final Long LEAD_SOURCE_ID = 2L;
    public static final DealerCostAndGross DEALER_COST_AND_GROSS = new DealerCostAndGross();

    static {
        DEALER_COST_AND_GROSS.setDealerId(DEALER_ID);
        DEALER_COST_AND_GROSS.setCost(200.0);
        DEALER_COST_AND_GROSS.setGross(300.0);
    }
}
```

### Step 2: Write JUnit Tests Using the Constants Class

#### Controller Test

For the `CostAndGrossController` class:

```java
import static org.springframework.test.web.servlet.request.MockMvcRequestBuilders.*;
import static org.springframework.test.web.servlet.result.MockMvcResultMatchers.*;
import static org.springframework.test.web.servlet.setup.MockMvcBuilders.*;
import static org.mockito.Mockito.*;

import org.junit.jupiter.api.BeforeEach;
import org.junit.jupiter.api.Test;
import org.mockito.InjectMocks;
import org.mockito.Mock;
import org.springframework.test.web.servlet.MockMvc;

public class CostAndGrossControllerTest {
    
    private MockMvc mockMvc;

    @Mock
    private DealerService dealerService;

    @InjectMocks
    private CostAndGrossController controller;

    @BeforeEach
    public void setup() {
        mockMvc = MockMvcBuilders.standaloneSetup(controller).build();
    }

    @Test
    public void testDeleteForLead() throws Exception {
        mockMvc.perform(delete("/api/path/{dealerId}/{leadSourceId}", TestData.DEALER_ID, TestData.LEAD_SOURCE_ID))
               .andExpect(status().isNoContent());

        verify(dealerService).deleteMethod(TestData.DEALER_ID, TestData.LEAD_SOURCE_ID);
    }
}
```

#### Service Test

For the `DealerService` class:

```java
import static org.mockito.Mockito.*;
import static org.junit.jupiter.api.Assertions.*;

import org.junit.jupiter.api.Test;
import org.mockito.InjectMocks;
import org.mockito.Mock;

public class DealerServiceTest {

    @Mock
    private CostAndGrossDetailsRepository repository;

    @InjectMocks
    private DealerService service;

    @Test
    public void testProcessDealerData() {
        when(repository.save(any())).thenReturn(TestData.DEALER_COST_AND_GROSS);
        assertDoesNotThrow(() -> service.processDealerData(TestData.DEALER_ID, TestData.DEALER_COST_AND_GROSS));
        verify(repository).save(TestData.DEALER_COST_AND_GROSS);
    }
}
```

#### Repository Test

Assuming you are using a repository like `CostAndGrossDetailsRepository`:

```java
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.boot.test.autoconfigure.orm.jpa.DataJpaTest;
import org.springframework.boot.test.autoconfigure.orm.jpa.TestEntityManager;
import org.junit.jupiter.api.Test;
import static org.assertj.core.api.Assertions.assertThat;

@DataJpaTest
public class CostAndGrossDetailsRepositoryTest {

    @Autowired
    private TestEntityManager entityManager;

    @Autowired
    private CostAndGrossDetailsRepository repository;

    @Test
    public void testFindByDealerId() {
        DealerCostAndGross savedEntity = entityManager.persistFlushFind(TestData.DEALER_COST_AND_GROSS);
        DealerCostAndGross foundEntity = repository.findById(TestData.DEALER_ID).orElse(null);
        assertThat(foundEntity).isEqualToComparingFieldByField(savedEntity);
    }
}
```

### Final Notes

1. **Setup and Mocking**: Ensure your setup and mocking are correctly handled. Use annotations like `@Mock` for mocking dependencies and `@InjectMocks` for injecting mocked services into your classes under test.
2. **Using Constants**: By using the `TestData` constants, you ensure consistency across tests and simplify the process of updating test values.
3. **Test Coverage**: These tests aim to cover major functionalities and interactions. You may need additional tests to cover edge cases and exception handling to reach over 90% coverage.

This approach helps maintain clean and organized tests that are easier to read and modify.
