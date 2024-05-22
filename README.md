import static org.springframework.test.web.servlet.request.MockMvcRequestBuilders.post;
import static org.springframework.test.web.servlet.result.MockMvcResultMatchers.content;
import static org.springframework.test.web.servlet.result.MockMvcResultMatchers.status;
import static org.mockito.Mockito.when;
import static org.hamcrest.Matchers.containsString;
import com.fasterxml.jackson.databind.ObjectMapper;
import org.springframework.http.MediaType;
import org.springframework.http.ResponseEntity;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.boot.test.autoconfigure.web.servlet.WebMvcTest;
import org.springframework.boot.test.mock.mockito.MockBean;
import org.springframework.test.web.servlet.MockMvc;

@WebMvcTest(CostAndGrossController.class)
public class CostAndGrossControllerTest {

    @Autowired
    private MockMvc mockMvc;

    @MockBean
    private DealerService dealerService;

    @Test
    void testSaveCostAndGrossForLeads() throws Exception {
        // Arrange
        Long dealerId = 123L;
        DealerCostAndGross dealerCostAndGross = new DealerCostAndGross();
        dealerCostAndGross.setDealerId(dealerId);
        Lead lead1 = new Lead();
        lead1.setLeadSourceId(1L);
        lead1.setCost(100.50);
        lead1.setGross(200.75);
        lead1.setLeadSourceName("Test");
        lead1.setMonth("5");
        lead1.setYear(2024);

        Lead lead2 = new Lead();
        lead2.setLeadSourceId(2L);
        lead2.setCost(150.25);
        lead2.setGross(250.00);
        lead2.setLeadSourceName("Test");
        lead2.setMonth("5");
        lead2.setYear(2024);

        List<Lead> leadList = Arrays.asList(lead1, lead2);
        dealerCostAndGross.setLeadSources(leadList);

        Pagination pagination = new Pagination();
        pagination.setLimit(10);
        pagination.setOffset(5);
        dealerCostAndGross.setPagination(pagination);

        // Mocking service to return a response entity with status 201 (Created) and body "Created"
        when(dealerService.processDealerData(dealerId, dealerCostAndGross))
                .thenReturn(ResponseEntity.status(HttpStatus.CREATED).body("Created"));

        // Act & Assert
        mockMvc.perform(post("/marketing/leads")
                .content(new ObjectMapper().writeValueAsString(dealerCostAndGross))
                .contentType(MediaType.APPLICATION_JSON))
                .andExpect(status().isCreated())
                .andExpect(content().string(containsString("Created")));
    }
}
