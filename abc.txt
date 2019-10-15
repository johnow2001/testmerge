package PageObjects;

import java.util.ArrayList;
import java.util.List;
import org.openqa.selenium.By;
import org.openqa.selenium.WebDriver;
import org.openqa.selenium.WebElement;
import org.openqa.selenium.support.FindAll;
import org.openqa.selenium.support.FindBy;
import org.openqa.selenium.support.How;
import org.openqa.selenium.support.PageFactory;
import org.openqa.selenium.support.ui.ExpectedConditions;
import org.openqa.selenium.support.ui.Select;
import org.openqa.selenium.support.ui.WebDriverWait;

public class DressPage {
    WebDriver driver;

    public DressPage(WebDriver driver) {
        this.driver = driver;
        PageFactory.initElements(driver, this);
    }

    //correct item as there are two prices
    @FindAll(@FindBy(how = How.CSS, using = ".right-block .content_price .price"))
    private List<WebElement> prices;

    @FindBy(how = How.ID, using = "selectProductSort")
    private WebElement sortBy;

    public String findMostExpensive() {

        //sort the page with highest price first
        Select sort = new Select(sortBy);
        sort.selectByValue("price:desc");

        List<String> itemPrices = new ArrayList<String>();
        for (WebElement el : prices) {
            itemPrices.add(el.getText());
        }

        System.out.println("Dress price at index 0 = " + itemPrices.get(0));

        // expected highest prices at index 0
        String gotHighestPrice = itemPrices.get(0);
        return gotHighestPrice;
    }

    public void addToCart() {

        driver.findElement(By.cssSelector("img[alt='Printed Dress']")).click();
        WebElement cartFrame = driver.findElement(By.tagName("iframe"));
        driver.switchTo().frame(cartFrame);
        driver.findElement(By.id("add_to_cart")).click();

        // continue to shop
        WebDriverWait wait = new WebDriverWait(driver, 10);
        wait.until(ExpectedConditions
                .elementToBeClickable(By.xpath("//*[@id=\"layer_cart\"]/div[1]/div[2]/div[4]/span/span")));
        driver.findElement(By.xpath("//*[@id=\"layer_cart\"]/div[1]/div[2]/div[4]/span/span")).click();
    }
}
