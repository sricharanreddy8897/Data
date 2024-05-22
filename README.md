import org.junit.jupiter.api.Test;

import java.util.Arrays;
import java.util.List;

import static org.junit.jupiter.api.Assertions.assertEquals;

public class DealerCostAndGrossTest {

    @Test
    public void testDealerCostAndGross() {
        // Create Lead objects
        Lead lead1 = new Lead();
        lead1.setLeadSourceId(1);
        lead1.setCost(100.50);
        lead1.setGross(200.75);

        Lead lead2 = new Lead();
        lead2.setLeadSourceId(2);
        lead2.setCost(150.25);
        lead2.setGross(250.00);

        List<Lead> leadList = Arrays.asList(lead1, lead2);

        // Create Pagination object
        Pagination pagination = new Pagination();
        pagination.setLimit(10);
        pagination.setOffset(5);

        // Create DealerCostAndGross object
        DealerCostAndGross dealerCostAndGross = new DealerCostAndGross();
        dealerCostAndGross.setDealerId(123);
        dealerCostAndGross.setLeadSources(leadList);
        dealerCostAndGross.setPagination(pagination);

        // Assert DealerCostAndGross values
        assertEquals(123, dealerCostAndGross.getDealerId());
        assertEquals(leadList, dealerCostAndGross.getLeadSources());
        assertEquals(pagination, dealerCostAndGross.getPagination());

        // Assert individual Lead values
        assertEquals(1, dealerCostAndGross.getLeadSources().get(0).getLeadSourceId());
        assertEquals(100.50, dealerCostAndGross.getLeadSources().get(0).getCost());
        assertEquals(200.75, dealerCostAndGross.getLeadSources().get(0).getGross());

        assertEquals(2, dealerCostAndGross.getLeadSources().get(1).getLeadSourceId());
        assertEquals(150.25, dealerCostAndGross.getLeadSources().get(1).getCost());
        assertEquals(250.00, dealerCostAndGross.getLeadSources().get(1).getGross());

        // Assert Pagination values
        assertEquals(10, dealerCostAndGross.getPagination().getLimit());
        assertEquals(5, dealerCostAndGross.getPagination().getOffset());
    }
}
