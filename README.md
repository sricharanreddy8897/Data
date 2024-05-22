import org.junit.jupiter.api.Test;

import java.sql.Timestamp;

import static org.junit.jupiter.api.Assertions.assertEquals;

public class DealerLeadSourceCostAndGrossEntityTest {

    @Test
    public void testDealerLeadSourceCostAndGrossEntity() {
        DealerLeadSourceCostAndGrossEntity entity = new DealerLeadSourceCostAndGrossEntity();
        
        // Set values
        entity.setId(1);
        entity.setInternalDealerId(123);
        entity.setModifiedLeadSource("Modified Source");
        entity.setMonth("2024-05");
        entity.setCost(100.50);
        entity.setGross(200.75);
        entity.setNet(300.25);
        entity.setNumberOfSales(10L);
        entity.setCreatedDate(Timestamp.valueOf("2024-05-15 10:00:00"));
        entity.setUserUpdated("User123");
        entity.setUpdatedDate(Timestamp.valueOf("2024-05-16 12:00:00"));
        entity.setVersionNumber(1);
        
        // Assert values
        assertEquals(1, entity.getId());
        assertEquals(123, entity.getInternalDealerId());
        assertEquals("Modified Source", entity.getModifiedLeadSource());
        assertEquals("2024-05", entity.getMonth());
        assertEquals(100.50, entity.getCost());
        assertEquals(200.75, entity.getGross());
        assertEquals(300.25, entity.getNet());
        assertEquals(10L, entity.getNumberOfSales());
        assertEquals(Timestamp.valueOf("2024-05-15 10:00:00"), entity.getCreatedDate());
        assertEquals("User123", entity.getUserUpdated());
        assertEquals(Timestamp.valueOf("2024-05-16 12:00:00"), entity.getUpdatedDate());
        assertEquals(1, entity.getVersionNumber());
    }
}
