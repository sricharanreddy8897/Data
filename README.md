import javax.persistence.*;

@Entity
@Table(name = "cost_and_gross_details")
public class CostAndGrossDetails {
    @Id
    @GeneratedValue(strategy = GenerationType.IDENTITY)
    @Column(name = "id")
    private Long id;

    @Column(name = "Lead_source_id")
    private int leadSourceId;

    @Column(name = "Source_name")
    private String sourceName;

    @Column(name = "Cost")
    private double cost;

    @Column(name = "Gross")
    private double gross;

    @Column(name = "Dealer_id")
    private int dealerId;

    @Column(name = "Reporting_month")
    private String reportingMonth;

    // Getters and setters
}
