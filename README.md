import java.io.Serializable;
import javax.persistence.*;

@Entity
@Table(name = "cost_and_gross_details")
public class CostAndGrossDetails {
    
    @Embeddable
    public static class CostAndGrossDetailsId implements Serializable {
        @Column(name = "Lead_source_id")
        private int leadSourceId;

        @Column(name = "Dealer_id")
        private int dealerId;

        @Column(name = "Reporting_month")
        private String reportingMonth;

        // Constructors, equals, and hashCode methods
    }
    
    @EmbeddedId
    private CostAndGrossDetailsId id;

    @Column(name = "Source_name")
    private String sourceName;

    @Column(name = "Cost")
    private double cost;

    @Column(name = "Gross")
    private double gross;

    // Getters and setters
}
