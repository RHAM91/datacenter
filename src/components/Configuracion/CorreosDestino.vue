<template>
    <b-container fluid>
        <b-row>
            <b-col sm="5" class="mt-3">
                <label>Nombre</label>
                <b-form-input type="text" size="sm" placeholder="Ej. Romario Torres"></b-form-input>
            </b-col>
            <b-col sm="6" class="mt-3">
                <label>Correo</label>
                <b-form-input type="email" size="sm" placeholder="@"></b-form-input>
            </b-col>
            <b-col sm="1" class="mt-5">
                <b-button type="button" size="sm" variant="success"><i class="fas fa-save"></i></b-button>
            </b-col>

            <b-col sm="12" class="mt-4">
                <table class="table table-sm table-bordered table-hover"  style="font-size: 12px;">
                    <thead>
                        <tr>
                            <th style="width: 40%;">
                                Nombre
                            </th>
                            <th style="width: 40%;">
                                Correo
                            </th>
                            <th style="width: 20%;text-align: center;">
                                ...
                            </th>
                        </tr>
                    </thead>
                    <tbody>
                        <tr>
                            <td>
                                Romario Torres
                            </td>
                            <td>
                                Correo
                            </td>
                            <td style="text-align: center;">
                                <b-button type="button" size="sm" variant="primary" style="margin-right: 5px;"><i class="fas fa-edit"></i></b-button>
                                <b-button type="button" size="sm" variant="danger"><i class="fas fa-trash-alt"></i></b-button>
                            </td>
                        </tr>
                    </tbody>
                </table>
            </b-col>
        </b-row>
    </b-container>
</template>

<script>
export default {

}
</script>

<style>

</style>