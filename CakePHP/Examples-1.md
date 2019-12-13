### Examples


Next scrip is used to copy and paste a new row in a table:
```php
<script>
<?php $this->Html->scriptStart(['block' => 'scriptBottom', 'inline' => false]); ?>
$(document).ready(function () {
    var competenceCount = <?= $competenceCount ?>;

    function addCompetenceRow() {
        var row = '<tr class="competence-row-id">';

        row += '<td><?= $this->Form->input("employee_employee_competences.row-id.employee_competence_id", [
            'empty' => true,
            'label' => false,
            'options' => $competences,
        ]) ?></td>';
        row += '<td><?= $this->Form->input("employee_employee_competences.row-id.expiration_date", [
            'empty' => true,
            'label' => false,
            'suffix' => '<span class="fa fa-calendar"></span>',
            'templateVars' => ['contentClass' => 'input-group date', 'groupClass' => 'form-group-date'],
            'type' => 'text',
        ]) ?></td>';
        row += '<td><?= $this->Form->input("employee_employee_competences.row-id.remarks", [
            'label' => false,
        ]) ?></td>';
        row += '<td class="actions"><?= $this->Form->button('<span class="fa fa-times"></span>', [
            'class' => 'btn btn-icon btn-delete-competence',
            'escape' => false,
            'row' => 'row-id',
            'type' => 'button',
        ]) ?></td>';
        row += '</tr>';

        $('#table-competences tbody').append(row.replace(new RegExp('row-id', 'g'), competenceCount));

        $('.form-group-employee-employee-competences-' + competenceCount + '-expiration-date .form-group-content.input-group').datepicker({
            autoclose: true,
            clearBtn: true,
            format: 'dd-mm-yyyy',
            language: 'nl-BE',
            todayHighlight: true
        });

        $('button.btn-delete-competence').click(function () {
            deleteCompetenceRow($(this).attr('row'));
        });

        competenceCount++;
    }

    function deleteCompetenceRow(rowId) {
        $('#table-competences tbody tr.competence-' + rowId).remove();
    }

    $('#add-competence').click(addCompetenceRow);

    $('button.btn-delete-competence').click(function () {
        deleteCompetenceRow($(this).attr('row'));
    });
});
<?php $this->Html->scriptEnd(); ?>
</script>
```