import java.util.ArrayList;
import java.util.List;

public class TestData {
    public static final Long DEALER_ID = 1L;

    // Setting up a list of Leads
    public static final List<Lead> LEAD_LIST = new ArrayList<>();
    static {
        Lead lead1 = new Lead();
        lead1.setLeadSourceId(101);
        lead1.setLeadSourceName("Online");
        lead1.setCost(500.0);
        lead1.setGross(1000.0);
        lead1.setMonth("May");
        lead1.setYear(2023);

        Lead lead2 = new Lead();
        lead2.setLeadSourceId(102);
        lead2.setLeadSourceName("Referral");
        lead2.setCost(300.0);
        lead2.setGross(700.0);
        lead2.setMonth("June");
        lead2.setYear(2023);

        LEAD_LIST.add(lead1);
        LEAD_LIST.add(lead2);
    }

    // Setting up Pagination
    public static final Pagination PAGINATION = new Pagination();
    static {
        PAGINATION.setCount(2);
        PAGINATION.setLimit(10);
        PAGINATION.setOffset(0);
    }

    // Setting up DealerCostAndGross
    public static final DealerCostAndGross DEALER_COST_AND_GROSS = new DealerCostAndGross();
    static {
        DEALER_COST_AND_GROSS.setDealerId(DEALER_ID);
        DEALER_COST_AND_GROSS.setLeadSources(LEAD_LIST);
        DEALER_COST_AND_GROSS.setPagination(PAGINATION);
    }
}




import static org.mockito.Mockito.*;
import static org.junit.jupiter.api.Assertions.*;

import org.junit.jupiter.api.Test;
import org.mockito.InjectMocks;
import org.mockito.Mock;

public class DealerServiceTest {

    @Mock
    private DealerRepository dealerRepository; // Assuming such a repository exists

    @InjectMocks
    private DealerService dealerService;

    @Test
    public void testProcessDealerData() {
        when(dealerRepository.save(any(DealerCostAndGross.class))).thenReturn(TestData.DEALER_COST_AND_GROSS);
        DealerCostAndGross result = dealerService.processDealerData(TestData.DEALER_COST_AND_GROSS);

        assertNotNull(result);
        assertEquals(TestData.DEALER_ID, result.getDealerId());
        assertTrue(result.getLeadSources().size() > 0);
        verify(dealerRepository).save(TestData.DEALER_COST_AND_GROSS);
    }
}





