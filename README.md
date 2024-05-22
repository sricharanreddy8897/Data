import org.junit.jupiter.api.Test;
import static org.junit.jupiter.api.Assertions.*;

class DealerLeadSourceCostAndGrossEntityTest {

    @Test
    void testLombokGeneratedMethods() {
        DealerLeadSourceCostAndGrossEntity entity = new DealerLeadSourceCostAndGrossEntity();
        
        entity.setId(1);
        entity.setInternalDealerId(123);
        entity.setModifiedLeadSource("online");
        entity.setMonth("January");
        entity.setCost(100.0);
        entity.setGross(200.0);
        
        assertEquals(1, entity.getId());
        assertEquals(123, entity.getInternalDealerId());
        assertEquals("online", entity.getModifiedLeadSource());
        assertEquals("January", entity.getMonth());
        assertEquals(100.0, entity.getCost());
        assertEquals(200.0, entity.getGross());
        
        // Test toString, equals, and hashCode if necessary
        String expectedToString = "DealerLeadSourceCostAndGrossEntity(id=1, internalDealerId=123, modifiedLeadSource=online, month=January, cost=100.0, gross=200.0)";
        assertEquals(expectedToString, entity.toString());
        
        DealerLeadSourceCostAndGrossEntity entity2 = new DealerLeadSourceCostAndGrossEntity();
        entity2.setId(1);
        entity2.setInternalDealerId(123);
        entity2.setModifiedLeadSource("online");
        entity2.setMonth("January");
        entity2.setCost(100.0);
        entity2.setGross(200.0);
        
        assertEquals(entity, entity2);
        assertEquals(entity.hashCode(), entity2.hashCode());
    }
}
